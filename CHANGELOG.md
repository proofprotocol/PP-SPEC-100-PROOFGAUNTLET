# Changelog

## 0.1.0 (2026-11-02)
- Initial draft. ProofGauntlet defined as a two-stage, deterministic measure of how much a registered proof can carry.
- Section 3: any record listed in a PP-SPEC-004 registry, starting with proofregister.com, is a Subject Record; Full and Legacy Records; non-public records; overlay, never edit; every result says where the proof is found.
- Section 4: Registry Profile. API first, open to machines (no bot walls, no robots.txt exclusion of agents), enumerable, verifiable, keys published elsewhere, browser-callable.
- Section 5: API first for the Gauntlet Service; endpoints; open, stateless, local computation; errors; agents and MCP.
- Section 6: the Gate, checks G1 to G9, verdicts `valid`, `valid_legacy`, `invalid`, `unverifiable`; no partial credit; fail closed.
- Section 7: the Strength Rating. Rating Inputs; Dimensions D1 Tier, D2 Adversarial Depth, D3 Environment Fidelity, D4 Capture Fidelity, D5 Independence, D6 Freshness; Levels 0 to 4; unknown is zero; Weights; Ceilings K1 to K3; Bands; strength is not efficacy; immutable, versioned Rubrics.
- Rubric `PGR-0.1.0` (rubric/PGR-0.1.0.json).
- Sections 8 to 12: Gauntlet Verdict; rankings with mandatory inclusion, selection disclosure, and no placement for sale; Gauntlet Verdict Record and Ranking Snapshot with tag `PSP-1:record`; human and machine forms; IP-G1 to IP-G5.
- schemas/gauntlet.schema.json, openapi/proofgauntlet.openapi.yaml, vectors/gauntlet-vectors-0.1.0.json.
- Licensed under the Proof Protocol License 1.1: specification text CC BY 4.0 from this first release; patents only under LICENSE.md Part A2 and PATENT-SCHEDULE.md; no trademark rights.
