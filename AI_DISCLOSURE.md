# AI Disclosure

*How this specification was made, and by whom.*

## Summary

ProofGauntlet is Craig Ellrod's idea. Claude proposed most of the rubric and drafted the words.

Craig Ellrod conceived ProofGauntlet: a place that tells a reader where a proof is found, whether it is valid, and how strong it is, so that "valid but weak" and "valid and extremely strong" can be told apart, and proofs can be ranked by strength. He named it, registered proofgauntlet.com, set its scope as any proof listed in proofregister.com, and required that both proofregister.com and ProofGauntlet be API first. Claude, an AI assistant made by Anthropic, proposed the two-stage structure, the six Dimensions, and the Levels, Weights, Ceilings, and Bands at his direction, drawing on concepts he had already published (the assurance ladder, Units of Proof, reproof, the Witness Coverage and absence rules of PP-SPEC-077), and drafted this specification, its rubric, schemas, API description, and conformance vectors.

## Ratio

| Dimension | Human | AI |
|---|---|---|
| Conception and every normative decision | 50% | 50% (the product concept, its scope, and API first are the author's; the Gate checks, Dimensions, thresholds, Weights, Ceilings, Bands, and ranking rules were proposed by Claude and accepted by him) |
| Design details and naming | 25% | 75% (the author named ProofGauntlet; Claude proposed: Proof Strength Protocol, the Gate, Strength Rating and Bands, Dimension names, Ceilings K1 to K3, Registry Profile, Locator, Rating Inputs, `PGR` rubric identifiers, `PSP-1` tag, IP-G1 to IP-G5) |
| Words, code, and schemas as written | 0% | 100% |
| Review, direction, acceptance, decision to publish | 100% | 0% |

Method: self-estimated (equivalent to self_attested, 077 21.15.4). MAY be re-measured with a tool such as ProofJack.com.

Machine-readable form: [ai_disclosure.json](ai_disclosure.json), conforming to PP-SPEC-077 Section 21.15. The two forms agree; if they ever differ, the JSON is wrong and must be corrected.

## This version (0.1.0, 2026-11-02)

**Human:**
- The ProofGauntlet concept: where a proof is found, whether it is valid, how strong it is, and a ranking of proofs by strength
- The name ProofGauntlet, the trademark, and the proofgauntlet.com domain
- Scope: any proof listed in proofregister.com
- API first for both proofregister.com and ProofGauntlet
- The assurance ladder, Units of Proof, and reproof the Dimensions build on
- Review and acceptance, 2026-11-02

**AI (Claude Opus 5.5):**
- Every section's text, the rubric file, the schema, the OpenAPI description, the conformance vectors, and the supporting documents

**Proposed by the AI, accepted by the author on 2026-11-02:**
- The six Dimensions and their Levels (7.3), the Weights (7.4), Ceilings K1 to K3 (7.5), and Bands (7.6)
- Every numeric threshold in `PGR-0.1.0`: 5 Units of Proof per technique, 25% novelty share, 10% Blind plus Void share, the 30, 90, 180, and 365 day Freshness steps, the technique counts of D2
- Treating PP-SPEC-004 records as rateable Legacy Records (3.2) rather than excluding them
- The Registry Profile requirements (Section 4), mandatory inclusion in rankings (9.2), and selection disclosure (9.3)
- The names and terms listed above

Nothing in this file changes the terms in NOTICE.md or LICENSE.md.
