# fallsieve · design note

> Spec: **fallsieve-spec-v1**. The engineering contract of the curation gate.

## Surface

`sieve.mjs` exports:

- `Sieve` — `new Sieve({ assess, minScore?, allow? })`, `sift(candidates) → { kept, rejected,
  flagged, summary }`.
- `classifyLicence(licence) → 'permissive' | 'copyleft' | 'unknown'`.
- `contentHash(content)` — FNV-1a hex content-address.
- `cluster(records)` — group records that share content.

## The two gates

A candidate `{ id?, content, licence?, source? }` is:

- **rejected** if the injected `assess` returns a score below `minScore`.
- **flagged** if it clears quality but its licence class is not in `allow` (default `['permissive']`)
  — copyleft and unknown licences need a human decision, never silent admission.
- **kept** if it clears both gates; deduped by content-address; provenance (source, licence, score,
  hash) attached.

## Invariants

1. **No verdict of its own.** The quality assessor is injected; the sieve only applies the threshold.
2. **Licence discipline.** A high score never overrides an inadmissible licence. Copyleft/unknown are
   flagged, not dropped and not silently kept.
3. **Provenance-complete.** Every kept record carries its source, licence, score, and content hash.
4. **Deterministic.** Same candidates + same assessor ⇒ same partition. `summary` counts reconcile to
   the input.
5. **Zero dependencies.**

## Verification

`npm test` — licence classification, content-address determinism + dedup, the reject/flag/keep gates,
policy opt-in for copyleft, async assessor, summary reconciliation, clustering. CI runs it on push.
