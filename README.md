# Korean Academic Research Digest

A twice-daily feed of new papers from 13 Korean universities and research institutes, sorted by subject.

**Live page:** https://kristenharim.github.io/Korean-Academic-RSS/ · **Feeds:** [`feed.xml`](feed.xml) (RSS) · [`feed.json`](feed.json)

## Why I built it

Following new work out of Korean institutions meant checking a dozen separate sites. PubMed already indexes most of it by author affiliation, so this project turns one saved affiliation search per institution into a single feed you can read in any RSS reader.

## How it works

1. **Sources.** One PubMed affiliation search per institution, read as RSS: KAIST, SNU, Yonsei, SKKU, POSTECH, Korea University, IBS, UNIST, Hanyang, KIST, GIST, Kyung Hee and Pusan National University.
2. **Dedupe and archive.** New papers are merged into [`papers-archive.csv`](papers-archive.csv), which keeps every paper seen so far (about 139,000 rows). Papers with authors at more than one institution are flagged as multi-institutional.
3. **Subject tagging.** Each title and abstract is tagged as Computer Science, Biomedical, Cancer Research, Materials Science, Neuroscience or Multidisciplinary. The generator tries a small TensorFlow.js model first and falls back to keyword rules (`ml-features.js`) when the model is unavailable, which is the usual case in CI. Every tag carries a confidence score.
4. **Publish.** A GitHub Actions job ([`generate-rss.yml`](.github/workflows/generate-rss.yml)) runs at about 12:30 AM and 12:00 PM Pacific, rewrites `feed.xml`, `feed.json` and [`stats.json`](stats.json), and commits them. That is why most commits here come from `github-actions`.

## Run it locally

```bash
npm install
node rss-generator-script.js
```

## Limits

- Coverage is whatever PubMed indexes, so it leans biomedical. Engineering and CS work that never reaches PubMed is missed.
- Affiliation matching depends on how authors write their institution, so some papers are missed or misattributed.
- The keyword rules are coarse. "Multidisciplinary" is the catch-all and is the largest bucket.
