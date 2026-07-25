# fallsieve

**Live:** [sjgant80-hub.github.io/fallsieve](https://sjgant80-hub.github.io/fallsieve/)

A **curation gate**. Feed it a set of candidate artifacts and it keeps only the ones that clear two
gates — quality and licence — discards the rest, dedupes survivors, and keeps their provenance.

- **Quality gate** — you inject the assessor (`assess(candidate) → { score }`), so it's
  benchmark-agnostic. Below your threshold, a candidate is dropped. The sieve never invents a verdict.
- **Licence gate** — a candidate you can't legally reuse isn't "kept" just because it scored well.
  Permissive licences pass; copyleft and unknown are **flagged for a human**, never silently admitted.

The honest version of "ingest open source and keep the good bits": curate through a gate, respect the
licence, and carry the provenance so nothing is admitted without a source.

## Use

```js
import { Sieve } from './sieve.mjs';

const sieve = new Sieve({ assess: myBenchmark, minScore: 0.7 });   // bring your own assessor
const { kept, rejected, flagged, summary } = await sieve.sift(candidates);
// candidate = { id?, content, licence?, source? }
```

## Test

```
npm test
```

Zero dependencies. Deterministic.
