# CLAUDE.md · fallsieve

Instructions for any agent working in this repository. See `SPEC.md` for the contract.

## What this is

A curation gate: candidates → quality gate (injected assessor) + licence gate → kept / rejected /
flagged, with provenance. The EXPLORE/selection engine of the estate, built public-clean.
`sieve.mjs` is the importable engine; `index.html` is a browser demo.

## Invariants to preserve

1. **The sieve computes no quality verdict of its own.** `assess` is injected and required. Do not
   hardcode a benchmark — that is the whole point of being benchmark-agnostic.
2. **Licence discipline is not optional.** A candidate that scores well but is copyleft or
   unknown-licensed is FLAGGED for a human, never auto-kept. Ingesting others' work without licence
   discipline is theft — never weaken this to boost the "kept" count.
3. **Provenance-complete.** Never admit a candidate without carrying its source/licence/score/hash.
4. **Deterministic.** No RNG, no clock. Same input ⇒ same partition.
5. **Zero dependencies.** A change that reddens `npm test` does not ship.

## Run
```
npm test
```
CI runs `npm test` on every push.

## Seam

Public, general-purpose. Curation / licence / provenance language only. Do NOT introduce private
estate cosmology (no κ/θ/Ψ, no "genome" mysticism, no element or dyad references) and do NOT frame it
as scraping/ingesting others' repos without licence checks — the licence gate exists precisely so this
tool stays on the right side of that line.
