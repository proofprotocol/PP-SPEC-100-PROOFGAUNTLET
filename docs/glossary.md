# Glossary

Normative definitions are in SPEC Section 2. Terms from PP-SPEC-077, 079, and 081 keep their meanings.

| Term | Meaning |
|---|---|
| As-Of Time | The instant a verdict or rating is computed for |
| Ceiling | A maximum score when a condition holds (K1 to K3) |
| Current / Historical | Whether a rating's record is still within validity and bound to a current Fingerprint |
| Dimension | One of six axes of strength: Tier, Adversarial Depth, Environment Fidelity, Capture Fidelity, Independence, Freshness |
| Full Record | A record carrying PP-SPEC-077 Section 21 fields |
| Gate | The pass-or-fail stage: is the record real? |
| Gate Verdict | `valid`, `valid_legacy`, `invalid`, or `unverifiable` |
| Gauntlet Service | An implementation of the API; the public instance is proofgauntlet.com |
| Gauntlet Verdict | The combined Gate and Strength Rating response for one record at one time |
| Legacy Record | A record carrying only the PP-SPEC-004 ProofRecord fields |
| Level | 0 to 4, per Dimension |
| Locator | Registry ID plus Record ID, e.g. `proofregister.com` / `PR-2026-00001` |
| Ranking Entry | One row per DUT: its strongest current proof, with the rest shown beside it |
| Ranking Set | Records selected by a declared filter for comparison |
| Rater | Anyone computing a verdict or rating |
| Rating Inputs | The normalized facts extracted from a record; the only input to the rubric |
| Registry Profile | What a registry must do for its records to be rated (SPEC Section 4) |
| Rubric | The versioned, immutable data file of Levels, thresholds, Weights, Ceilings, and Bands; `PGR-0.1.0` first |
| Source Registry | A PP-SPEC-004 registry records are resolved from; proofregister.com first |
| Strength Band | `weak`, `moderate`, `strong`, `extremely_strong` |
| Strength Rating | 0 to 100, how much weight a valid record can carry |
| Subject Record | The record being gated or rated |
| Weight | A Dimension's share of the score; Weights sum to 100 |
