# ProofGauntlet™: The Proof Strength Protocol (PSP)

*A Proof Protocol specification*

> **Ownership and Attribution Notice**
>
> This specification is authored by **Craig Thomas Ellrod** and is published as part of **Proof Protocol**. Copyright (c) 2026 Craig Thomas Ellrod.
>
> Any use, implementation, derivative, citation, or incorporation of this specification, in whole or in part, MUST attribute **Craig Thomas Ellrod and Proof Protocol**. This requirement applies to standards bodies, working groups, consortia, and commercial implementations.
>
> This specification is licensed under the Proof Protocol License 1.1 (see LICENSE.md); text CC BY 4.0 under Part A, which grants no patent rights and no trademark rights. Original inventor: Craig Thomas Ellrod (ORCID 0009-0004-1058-7666).
>
> **Patents.** Patent terms are in [PATENT-SCHEDULE.md](PATENT-SCHEDULE.md). Claims not listed there are reserved by Craig Thomas Ellrod. Licensing: 1proofprotocol@gmail.com.
>
> **Trademarks.** ProofGauntlet™ and Proof Gauntlet™ are trademarks of Craig Thomas Ellrod. Use of this specification grants no right to use them.

| Field | Value |
|---|---|
| Spec ID | PP-SPEC-100 |
| Version | 0.1.0 (Draft) |
| Date | 2026-11-02 |
| Status | Draft for public record |
| Maintainer | Craig Thomas Ellrod (Proof Protocol) |
| Builds on | PP-SPEC-004 (ProofRegister API), PP-SPEC-077 v0.8.3 (Dead Canary), PP-SPEC-078 (Jailed Canary), PP-SPEC-079 v0.1.4 (Coal Mine), PP-SPEC-080 (Pithead), PP-SPEC-081 v0.1.0 (Reproof) |
| Public instance | https://proofgauntlet.com/api/v1 |

---

## Abstract

A registry answers two questions about a proof: does this record exist, and is it the record that was made? It does not answer the question every reader asks next: how much can this proof carry?

Two records can both be sealed, signed, anchored, and registered, and still differ enormously in what they establish. One is a single technique fired at a vendor's development box a year ago, adjudicated by the party that generated the attack. The other is a campaign of techniques across tactics, run in a customer-like environment last week, watched by an independent witness, and reproven by a second party with no stake in the first. Today both show up the same way: valid.

ProofGauntlet defines two stages that any record listed in a PP-SPEC-004 registry can be run through, starting with the public instance, proofregister.com. **The Gate** decides whether the record is real: `valid`, `valid_legacy`, `invalid`, or `unverifiable`. **The Strength Rating** scores a record that passed the Gate from 0 to 100 across six published Dimensions, with published Levels, Weights, and Ceilings. Both are computed from the record, its schema, and the Registry alone, so a person, a machine, and any independent implementation reach the same answer. A record that fails the Gate is never scored.

ProofGauntlet is API first. The API is the product; a website is one client of it. The same rule binds every registry ProofGauntlet reads from.

A proof can be valid and still be weak. This specification makes the difference visible.

## Introduction: Why This Specification Exists

### The problem this family of specifications solves

Nobody can price security risk, because nobody can measure security efficacy. Every control is described by the party that made it, and every reader of that description is relying on the word of someone with a stake in the answer. The Proof Protocol specifications replace description with proof. PP-SPEC-077 measures what a security control did to committed test traffic. PP-SPEC-078 measures whether an agent containment control stopped an agent that tried to break its policy. PP-SPEC-079 makes those measurements independent of whoever built the control, and comparable across controls of any origin. PP-SPEC-080 lets the parties who pay for risk state in advance what proof they will accept. PP-SPEC-081 makes any proof reproducible by a party that was not there. PP-SPEC-004 is where all of it is registered.

**This specification's part:** a deterministic, published measure of how much a registered proof can carry, and a place to compare proofs by that measure.

### Why this specification

Once proofs exist, they will be cited. A vendor will put one in a sales deck, a buyer in a procurement file, an underwriter in a submission, an agent in a trust handshake. Each citation will say the same thing: "we have proof." Without a measure of strength, the weakest conforming proof and the strongest are cited with the same word, and the market learns to treat the word as meaningless. That is how attestation lost its value. Proof must not go the same way.

The usual answer is a rating agency: someone looks at the evidence and gives an opinion. An opinion is the thing Proof Protocol exists to replace. A rating produced by a hidden method, a panel, or a model that cannot be re-run is another claim by another party with a stake in the answer.

ProofGauntlet is not an opinion. Its rubric is a versioned, published data file. Every Level is decided by fields the record already carries. Every Weight and Ceiling is stated. Any party can recompute any rating, and two parties that compute it differently have found a defect in one of the two implementations, not a difference of view.

**Scope.** This specification defines how any record listed in a PP-SPEC-004 registry is resolved, gated, rated, rendered, and ranked, the API that exposes those functions, and the requirements a registry must meet for its records to be rated. It does not change how any record is produced or registered.

**What this is not.** It is not record production: it creates no Units of Proof and runs no tests. It is not acceptance: whether a reader accepts a record is PP-SPEC-080, which MAY cite a Strength Band as one of its requirements. It is not reproof: independent re-execution is PP-SPEC-081, whose result this specification reads. It is not a statement about the control: a strong proof of a control that missed half its attacks is a strong proof that the control misses half its attacks. It is not a value: no Strength Rating is a price, a Unit of Proof weighting at the level of the record, or a basis for any value figure.

## 1. Conventions

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, NOT RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in BCP 14 (RFC 2119, RFC 8174) when, and only when, they appear in all capitals.

Section references of the form "077 9.4" refer to PP-SPEC-077; "004", "078", "079", "080", and "081" refer to those specifications in the same way. Unprefixed references are to this specification.

## 2. Terminology

Terms defined in PP-SPEC-077 Section 2, PP-SPEC-079 Section 2, and PP-SPEC-081 Section 2 have the same meaning here. In addition:

- **Subject Record:** The record being gated or rated. Any record a Source Registry lists, identified by its Locator.
- **Source Registry:** A PP-SPEC-004 registry from which a Subject Record is resolved. The first Source Registry is proofregister.com.
- **Locator:** The pair (Registry ID, Record ID) that identifies a Subject Record, where the Registry ID is the registry's host name (for example `proofregister.com`) and the Record ID is the identifier the registry returns (`PR-2026-NNNNN`, or a 077 21.1 content-derived Record ID).
- **Full Record:** A Subject Record carrying the fields of 077 Section 21 and the run records its result record references.
- **Legacy Record:** A Subject Record carrying only the PP-SPEC-004 ProofRecord object (004 Section 4), without 077 Section 21 fields.
- **Gate:** The pass-or-fail stage that decides whether a Subject Record is real (Section 6).
- **Gate Verdict:** The result of the Gate: `valid`, `valid_legacy`, `invalid`, or `unverifiable`.
- **Rating Inputs:** The normalized set of facts a Rater extracts from a Subject Record and the Registry (Section 7.2). The only input to the Rubric.
- **Rubric:** The versioned, published data file that maps Rating Inputs to Levels, and Levels to a Strength Rating (Section 7). The Rubric for this version is `PGR-0.1.0` ([rubric/PGR-0.1.0.json](rubric/PGR-0.1.0.json)).
- **Dimension:** One of the six axes of strength: Tier, Adversarial Depth, Environment Fidelity, Capture Fidelity, Independence, Freshness.
- **Level:** An integer from 0 to 4 assigned to one Dimension by the Rubric.
- **Weight:** The share of the Strength Rating carried by one Dimension. Weights sum to 100.
- **Ceiling:** A maximum the Strength Rating cannot exceed when a stated condition holds (7.5).
- **Strength Rating:** The integer from 0 to 100 computed under Section 7. Never assigned to a record whose Gate Verdict is `invalid` or `unverifiable`.
- **Strength Band:** The named range a Strength Rating falls in: `weak`, `moderate`, `strong`, `extremely_strong` (7.6).
- **As-Of Time:** The instant at which a Gate Verdict or Strength Rating is computed. Freshness, validity, and Reproof Status depend on it.
- **Current / Historical:** A rating is Current when its Subject Record is within its validity window (077 21.8) and its Fingerprints are not stale; otherwise Historical.
- **Rater:** Any party or system computing a Gate Verdict or Strength Rating under this specification.
- **Ranking Set:** The set of Subject Records selected by a declared filter for comparison (Section 9).
- **Ranking Entry:** The row of a Ranking Set for one DUT, showing its strongest Current rating and the history around it (9.3).
- **Gauntlet Service:** An implementation that exposes the functions of this specification over the API of Section 5. The public instance is proofgauntlet.com.

### 2.1. Relationship to other specifications

| Specification | What it produces | What this specification adds |
|---|---|---|
| PP-SPEC-004 ProofRegister | Where records live and how they are queried | The Registry Profile a registry must meet for its records to be rated (Section 4) |
| PP-SPEC-077 Dead Canary | Proofs of efficacy, Units of Proof, run metrics | The Gate checks of 077 Sections 9, 11, 14, 21; the Dimensions that read them |
| PP-SPEC-078 Jailed Canary | Bookings and verdicts for an Agent Under Test | Rated as efficacy records; offense outcomes count as adjudicated outcomes |
| PP-SPEC-079 Coal Mine | Engagements, Environment Classes, Parity Declarations | Environment Fidelity and Independence Levels |
| PP-SPEC-080 Pithead | Whether a reader accepts a record | Nothing. A profile MAY require a minimum Strength Band; this specification never decides acceptance |
| PP-SPEC-081 Reproof | Reproof Status, computed from the Registry | Independence Levels 3 and 4, and a Ceiling on divergence |

## 3. Scope: Any Registered Proof

3.1. **Any record, starting with proofregister.com.** Every record listed in a Source Registry is a Subject Record. A Gauntlet Service MUST be able to gate any record listed by proofregister.com, and MAY add other registries that meet Section 4. No submission, consent, or payment by the Holder or the Provider is required for a public record to be gated or rated.

3.2. **Full and Legacy Records.** A Full Record is gated by every check of Section 6 and rated on every Dimension. A Legacy Record is gated by the checks a PP-SPEC-004 ProofRecord can support and rated on the Dimensions its fields determine; every other Dimension is Level 0 with reason `not_in_record` (7.3.7). Legacy Records are not excluded. They are rated honestly, which means low.

3.3. **Non-public records.** For a record whose Disclosure Level is not `public` (077 21.9), the Gate performs only the checks the public envelope supports (existence, receipt, revocation; 077 21.9.2). The Gate Verdict is `unverifiable` with reason `undisclosed`, unless the Holder discloses the record to the Rater, in which case the rating MUST carry `disclosed_to_rater: true` and MUST NOT be published at a Disclosure Level more open than the Holder permits.

3.4. **Overlay, never edit.** Nothing in this specification edits, re-signs, revokes, re-registers, or annotates a Subject Record. Gate Verdicts and Strength Ratings are computed, like Reproof Status (081 9.1), and are never written into the record they describe. A record's existing fields, including the PP-SPEC-004 `pes_score`, are never overwritten, replaced, or merged with a Strength Rating; a rendering that shows both MUST label each by name.

3.5. **Where it is found.** Every Gate Verdict and Strength Rating MUST carry the Subject Record's Locator, its registry URI, its Registry Receipt where one exists, its Disclosure Level, and its Holder identifier. A rating that does not say where its proof is found is non-conforming.

## 4. Registry Profile

A Gauntlet Service reads only from registries that meet this profile. The profile adds to PP-SPEC-004 conformance (004 Section 9); it relaxes nothing in it.

4.1. **API first.** Every function the registry offers on a website MUST be available through its API, and the website MUST use only the public API. No record, field, or listing may be available to a browser and unavailable to a program.

4.2. **Open to machines.** Query endpoints MUST require no authentication (004 Section 2), MUST NOT be placed behind a CAPTCHA, browser check, JavaScript challenge, or any other test that distinguishes people from programs, and MUST NOT be disallowed to agents by `robots.txt`. An agent is a consumer like any other (077 21.14.5). Rate limits MAY be applied, and MUST be stated in response headers.

4.3. **Enumerable.** The list endpoint (`GET /records`, 004 Section 6) MUST support complete enumeration of every listed record, including revoked ones, through a stable pagination cursor and a `since` parameter returning records registered or revoked after a given time. "Any record listed" is only meaningful if the list can be read to the end.

4.4. **Verifiable.** The registry MUST expose `GET /records/:id/verify` (004 Section 3) and SHOULD expose, for every Full Record, the Registry Receipt (077 21.5) and the signature envelope (077 21.4) in the record response.

4.5. **Keys elsewhere.** Verification keys for the registry's own receipts MUST be published at a location other than the registry's API responses, so that a verifier never trusts a key only because the record it verifies supplied it.

4.6. **Browser-callable.** Query responses MUST carry CORS headers permitting `GET` from any origin, so that a record can be verified entirely inside a reader's browser (5.5).

4.7. **Non-conforming registries.** A record from a registry that does not meet this profile MAY still be gated if the record and its receipt are supplied directly (5.4), but its Gate Verdict MUST carry `registry_profile: non_conforming`, and it MUST NOT appear in any Ranking Set.

## 5. API First

5.1. **The API is the product.** Every function of this specification, resolving, gating, rating, ranking, and reading the Rubric, MUST be available through the API of this section before, or at the same time as, any user interface for it. A user interface MUST be a client of the public API and MUST NOT use any private endpoint or capability.

5.2. **Base and version.** The public instance base URL is `https://proofgauntlet.com/api/v1`. All endpoints are HTTPS. Paths are versioned; a breaking change is a new major path version, and the earlier version remains available for at least twelve months after the new one is published.

5.3. **Endpoints.**

| Method | Path | Description |
|---|---|---|
| GET | /registries | Source Registries this service resolves, with their Registry Profile status |
| GET | /gate/{registry_id}/{record_id} | Gate Verdict for a listed record, with every check and its evidence |
| POST | /gate | Gate Verdict for a record supplied in the request body (stateless) |
| GET | /rating/{registry_id}/{record_id} | Strength Rating for a listed record. Query: `as_of`, `rubric` |
| POST | /rating | Strength Rating for a record, or for Rating Inputs, supplied in the request body (stateless) |
| GET | /rankings | A Ranking Set. Query: `profile`, `technique`, `tactic`, `dut`, `environment_class`, `as_of`, `rubric`, `cursor` |
| GET | /rubrics | Every published Rubric version |
| GET | /rubrics/{version} | One Rubric, byte for byte the published data file |
| GET | /ratings/{rating_record_id} | A registered Rating Record (Section 10) |
| GET | /openapi.json | The OpenAPI 3.1 description of this API |

The machine description of these endpoints is [openapi/proofgauntlet.openapi.yaml](openapi/proofgauntlet.openapi.yaml). Response bodies conform to [schemas/gauntlet.schema.json](schemas/gauntlet.schema.json).

5.4. **Open reads.** Every `GET` endpoint MUST require no authentication and MUST meet 4.2 and 4.6. `POST /gate` and `POST /rating` MUST require no authentication.

5.5. **Stateless and local.** `POST /gate` and `POST /rating` MUST NOT store, log, index, or forward the request body beyond the time needed to answer, unless the caller sets `store=true`. Because the Rubric is published data and every check is defined here, a client MUST be able to compute any Gate Verdict and Strength Rating locally, including in a browser, without sending the record anywhere. A Gauntlet Service MUST NOT require a record to be uploaded to be rated.

5.6. **Errors.** Errors MUST be returned as `application/problem+json` (RFC 9457). A Source Registry that cannot be reached produces Gate Verdict `unverifiable` with reason `registry_unreachable`, never `invalid` and never a cached `valid`.

5.7. **Agents.** A Gauntlet Service MAY expose its functions to agents through an MCP server or similar interface. Any such interface MUST be a client of the public API and MUST expose no function the API lacks.

5.8. **Same answer.** A response from the API, a local computation, and the human rendering of Section 11 MUST agree for the same Subject Record, Registry state, As-Of Time, and Rubric (077 21.14.4).

## 6. Stage 1: The Gate

6.1. **Checks.** The Gate applies these checks, in this order, to the Subject Record. Each check produces `pass`, `fail`, `not_applicable`, or `unverifiable`, with the evidence that decided it.

| ID | Check | Full Record | Legacy Record |
|---|---|---|---|
| G1 | **Registered.** The Locator resolves in the Source Registry, and its Registry Receipt verifies (077 21.5), or `GET /records/:id/verify` succeeds (004) | Required | Required |
| G2 | **Not revoked.** No Revocation is registered against the record (077 21.6; 004 `status: active`) | Required | Required |
| G3 | **Intact.** The Record ID recomputes from the record's canonical form (077 21.1, 21.2), and the commitment root recomputes (077 9.5); for a Legacy Record, `root_hash` recomputes from the ProofBundle where available, else G3 relies on G1 and records `bundle_unavailable` | Required | Required |
| G4 | **Anchored.** The Time Anchor pulse the record cites (077 IP-2; 004 `nist_pulse_index`, `nist_output_value`) exists at the source beacon with the stated output, and its time is not after the registration time | Required | Required |
| G5 | **Signed.** Every required signature verifies (077 21.4) against a key resolved from a source other than the Subject Record (4.5) | Required | `not_applicable` |
| G6 | **Schema.** The record conforms to the published schema of the specification that produced it | Required | `not_applicable` |
| G7 | **Committed first.** The commitment was anchored to a Time Anchor pulse earlier than the first ingress record of the run (077 Section 9), and the reveal reproduces the root (077 11.3) | Required | `not_applicable` |
| G8 | **Valid run.** The run meets 077 11.3 (good canary pass rate and void share within declared thresholds) and closed accounting (077 14.3.2) | Required | `not_applicable` |
| G9 | **Conformant.** The run's Conformance Level is at least Conformant (077 21.13) | Required | `not_applicable` |

6.2. **Verdicts.**

| Gate Verdict | Rule |
|---|---|
| `invalid` | Any check is `fail` |
| `unverifiable` | No check is `fail`, and any Required check is `unverifiable` |
| `valid` | Full Record, and G1 to G9 all `pass` |
| `valid_legacy` | Legacy Record, G1 to G4 `pass`, and G5 to G9 `not_applicable` |

6.3. **No partial credit.** A record whose Gate Verdict is `invalid` or `unverifiable` has no Strength Rating. A rendering MUST show the failed or unverifiable checks and MUST NOT show a score, a band, a bar, a rank, or any Dimension Level for it.

6.4. **Fail closed.** A check that cannot be performed is `unverifiable`, never `pass`. A Rater MUST NOT reuse a cached `pass` for G1 or G2 older than the As-Of Time it reports.

6.5. **Invalid is published.** A Gauntlet Service MUST return the Gate Verdict for an `invalid` public record on request, with its evidence, exactly as it returns a `valid` one.

## 7. Stage 2: The Strength Rating

7.1. **Determinism.** A Strength Rating is a function of the Rating Inputs and the Rubric only. Two Raters applying the same Rubric version to the same Subject Record, Registry state, and As-Of Time MUST produce the same Levels, the same Ceilings, and the same Strength Rating. A Rater MUST NOT use any input not listed in 7.2, any judgment, model inference, panel, or vote.

7.2. **Rating Inputs.** The Rater extracts the following from the Subject Record, the run records it references, and the Registry. Each input is recorded with the field or record it came from. An input the records do not establish is `null` with a reason: `not_in_record`, `undisclosed`, or `not_applicable`.

| Input | Source |
|---|---|
| `record_kind` | `full` or `legacy` |
| `adjudicated_outcomes` | Whether the run adjudicated bad canary outcomes under 077 11.1 (or 078 verdicts) |
| `control_evidence_independent` | For a record without adjudicated outcomes: whether evidence that the control is present or fired was captured by a party other than the Provider |
| `efficacy_rate_with_fbr` | Whether Efficacy Rate is published with False Block Rate (077 12) |
| `gold_claims_tested` | Whether Gold canaries were run and Claim Accuracy Rate published (077 5.6.4, 12) |
| `novelty_share` | Mutated plus holdout bad canaries over all bad canaries (077 Section 6) |
| `qualifying_techniques` | Count of distinct ATT&CK or ATLAS technique IDs, each with at least `min_uop_per_technique` Units of Proof, in a valid run |
| `qualifying_tactics` | Count of distinct tactics those techniques span |
| `campaign_stages` | Largest number of sequential stages in one declared multi-stage path (077 5.6.5) |
| `environment` | 079 Environment Class, Engagement Mode, Parity Declaration dimensions, and 077 Operating Mode, or `undeclared` |
| `stream_degraded` | For Production Stream Mode, whether the series is `degraded` (077 16.2.5) at the As-Of Time |
| `witness_record` | Whether a signed Proof Witness record exists from a party other than the Provider (077 IP-6) |
| `coverage_published` | Whether the Witness Coverage Map and the Blind and Void shares are published (077 14.1, 14.3.3) |
| `blind_void_share` | Blind share plus Void share over released canaries |
| `witness_calibration_current` | Whether Witness Calibration (077 15.5) is current for the run |
| `boundary_capture_independent` | Whether the Ingress Observer (077 IP-4) and Fingerprint Capture (077 IP-5) were operated by parties other than the Provider and the Issuer, with anchored ingress records |
| `party_independence` | From the Party Set (081 2): whether the Provider or an Affiliate occupies any seam; whether Issuer, Proof Witness, and Adjudicator are mutually independent |
| `reproof_status` | Reproof Status with scope, count, and history (081 Sections 9, 10) at the As-Of Time |
| `anchor_time` | Time of the Time Anchor pulse that binds the result; for Production Stream Mode, of the latest non-degraded clutch |
| `historical` | Whether the record is past `valid_through` or bound to a stale Fingerprint (077 21.8, 15.2) at the As-Of Time |

7.3. **Dimensions and Levels.** For each Dimension the Level is the highest Level whose conditions all hold. Numeric thresholds are the Rubric's; the values below are those of `PGR-0.1.0`.

7.3.1. **D1 Tier: what was proven.**

| Level | Condition |
|---|---|
| 0 | Verification evidence only: self-attestation, Provider telemetry, a log or alert produced by the DUT, a Proof of Correctness alone (077 Section 3), or not determinable |
| 1 | Proof of Control: `control_evidence_independent` is true, and there are no adjudicated outcomes |
| 2 | Proof of Efficacy: `adjudicated_outcomes` and `efficacy_rate_with_fbr` are true |
| 3 | Level 2, and `gold_claims_tested` is true |
| 4 | Level 3, and `novelty_share` is at least 0.25 |

7.3.2. **D2 Adversarial Depth: how hard it was pushed.** `min_uop_per_technique` is 5.

| Level | Condition |
|---|---|
| 0 | `qualifying_techniques` is 0 |
| 1 | 1 |
| 2 | 2 to 4 |
| 3 | 5 to 11, or `campaign_stages` at least 3 |
| 4 | 12 or more, across at least 3 `qualifying_tactics` |

7.3.3. **D3 Environment Fidelity: how close to the real thing.**

| Level | Condition |
|---|---|
| 0 | `undeclared`, or Provider-Run Mode (079 4.3) |
| 1 | `dev`, or `lab-replica` built from a Reference Environment with no Implementor (079 5.4) |
| 2 | `qa`, or `lab-replica` from an Implementor Parity Declaration with any dimension `absent` |
| 3 | `pre-production`, or `lab-replica` from an Implementor Parity Declaration with no dimension `absent` |
| 4 | `production` In-Situ (079 4.1), or Production Stream Mode (077 16.2) with `stream_degraded` false |

7.3.4. **D4 Capture Fidelity: how honest the evidence was when it was captured.**

| Level | Condition |
|---|---|
| 0 | `witness_record` is false |
| 1 | `witness_record` is true, `coverage_published` is false |
| 2 | `coverage_published` is true |
| 3 | Level 2, `blind_void_share` at most 0.10, and `witness_calibration_current` is true |
| 4 | Level 3, and `boundary_capture_independent` is true |

7.3.5. **D5 Independence: who stood to gain.**

| Level | Condition |
|---|---|
| 0 | Not determinable, or the Provider or an Affiliate occupies any seam |
| 1 | The Provider occupies no seam, but the Issuer and the Proof Witness, or the Issuer and the Adjudicator, are the same party or Affiliates |
| 2 | Issuer, Proof Witness, and Adjudicator are mutually independent, and none is the Provider or its Affiliate |
| 3 | Level 2, and Reproof Status is `reproven` with count 1 |
| 4 | Level 2, and Reproof Status is `reproven` with count at least 2, or an Umpire Record with ruling `head_upheld` |

7.3.6. **D6 Freshness: whether it is still true.** Age is the As-Of Time minus `anchor_time`.

| Level | Condition |
|---|---|
| 0 | Age more than 365 days, or `historical` is true |
| 1 | Age at most 365 days |
| 2 | At most 180 days |
| 3 | At most 90 days |
| 4 | At most 30 days |

7.3.7. **Unknown is zero.** A Dimension whose deciding inputs are `null` takes Level 0, and the rating MUST record which inputs were `null` and why. An unknown is never assumed favorable.

7.4. **Score.** With Weights `w` and Levels `L`:

```
S = round_half_up( sum over d of ( w_d * L_d / 4 ) )
Strength Rating = min( S, every Ceiling that applies )
```

`PGR-0.1.0` Weights: D1 Tier 25, D2 Adversarial Depth 20, D3 Environment Fidelity 15, D4 Capture Fidelity 15, D5 Independence 15, D6 Freshness 10.

7.5. **Ceilings.**

| ID | Condition | Maximum |
|---|---|---|
| K1 | D1 Tier at Level 0 or 1 | 40 |
| K2 | Reproof Status `divergent`, `disputed`, or `not_upheld` | 50 |
| K3 | Gate Verdict `valid_legacy` | 40 |

A rating MUST list every Ceiling that applied, whether or not it lowered the score.

7.6. **Bands.**

| Strength Band | Strength Rating |
|---|---|
| `weak` | 0 to 39 |
| `moderate` | 40 to 69 |
| `strong` | 70 to 89 |
| `extremely_strong` | 90 to 100 |

7.7. **Strength is not efficacy.** A Strength Rating measures how much weight a record can carry, not how well the control performed. A rendering of a Strength Rating for an efficacy record MUST show, beside it, the record's Efficacy Rate, False Block Rate, and Blind and Void shares (077 12, 14.3.3). A Strength Rating MUST NOT be presented as a rating of the DUT, the Provider, or the product.

7.8. **Reproof history.** A rating that uses Reproof Status MUST carry its scope, count, and the counts of Agree, Diverge, Insufficient, abandoned, and lapsed Check Assays (081 10.4).

7.9. **Rubric versions.** A Rubric is identified as `PGR-<major>.<minor>.<patch>`, published as a data file in this repository and at `GET /rubrics/{version}`, and never edited after publication. A change to any Level condition, threshold, Weight, Ceiling, or Band is a new Rubric version, recorded in CHANGELOG.md with its reason. Every rating MUST carry the Rubric version it was computed under. A Gauntlet Service MUST be able to compute ratings under every published Rubric version. A rating computed under an earlier version is not rewritten when a new version is published.

7.10. **Weighting belongs to the consumer.** Under 081 9.7, any weighting of records by status belongs to the consumer and MUST be stated by the consumer. The Rubric is that statement for this specification. A Rater that uses any other weighting is not computing a Strength Rating and MUST NOT call its result one.

## 8. Gauntlet Verdict

8.1. **Contents.** A Gauntlet Verdict is the combined response for one Subject Record at one As-Of Time: the Locator and the fields of 3.5; the Gate Verdict with every check, result, and evidence pointer; and, where the Gate Verdict is `valid` or `valid_legacy`, the Rating Inputs, each Dimension's Level with its deciding inputs, the Weights, the Ceilings applied, the Strength Rating, the Strength Band, the efficacy metrics of 7.7, the reproof history of 7.8, whether the rating is Current or Historical, the Rubric version, and the As-Of Time.

8.2. **Machine form.** A Gauntlet Verdict conforms to the `gauntlet_verdict` definition in [schemas/gauntlet.schema.json](schemas/gauntlet.schema.json).

## 9. Rankings

9.1. **Ranking Sets.** A Ranking Set is defined by a declared filter: Technology Profile (077 Section 17), and optionally technique, tactic, DUT, Environment Class, As-Of Time, and Rubric version. A Ranking Set MUST NOT mix Technology Profiles unless the filter declares it.

9.2. **Inclusion.** A Ranking Set MUST include every public record in its Source Registries that matches its filter and has a Current rating. A Holder or Provider MUST NOT be able to remove a public record from a Ranking Set, opt a product out, or limit a Ranking Set to records it chose. Historical ratings, records from registries that do not meet Section 4, and records that are not public are not ranked; they are counted (9.3).

9.3. **Entries.** A Ranking Entry is one row per DUT. It shows the DUT's strongest Current rating, its Locator, and its Strength Band, and MUST also show, for the same DUT within the filter: the count of other Current rated records and their median Strength Rating; the counts of `invalid`, `unverifiable`, revoked, and non-public records; and the Efficacy Rate and False Block Rate of the ranked record. A best record shown without the rest is non-conforming. This is the rule that makes cherry-picking visible.

9.4. **Order.** Entries are ordered by Strength Rating, descending; ties by D5 Independence Level, then D6 Freshness Level, descending; then by Record ID in ascending byte order. The order is fully determined by the records and the Rubric.

9.5. **No placement for sale.** No payment, funding, sponsorship, membership, or commercial relationship may change whether a record is included, its rating, or its position (Proof Economy Code of Ethics, Section 7.4: capital may finance the production of proof but cannot purchase the truth of that proof). A Gauntlet Service MUST disclose its own funding under FUNDING.md rules.

9.6. **A ranking is not a product rating.** A Ranking Set orders proofs by strength. It MUST be labeled as such and MUST NOT be presented as a ranking of products, Providers, or controls by quality.

## 10. Records

10.1. **Optional registration.** Because every Gauntlet Verdict is recomputable, registration is not required. A Gauntlet Verdict or Ranking Snapshot MAY be registered as a record under 077 Section 21 for citation. Record IDs for this specification's record types are computed as `SHA-256("PSP-1:record" || canonical_form)` under the exclusions of 077 21.1.1.

10.2. **Record types.** **Gauntlet Verdict Record** (Section 8, signed by the Rater) and **Ranking Snapshot** (Section 9, the filter, As-Of Time, Rubric version, and every Entry, signed by the Rater). Their machine form is in [schemas/gauntlet.schema.json](schemas/gauntlet.schema.json).

10.3. **Rater disclosure.** A registered record MUST carry the Rater's identity and `disclosed_interests` (077 2). A Rater that is the Provider, the Holder, or an Affiliate of either MAY register a Gauntlet Verdict, and the record MUST say so. Because the rating is deterministic, who computed it changes nothing about its value, only about who vouches for the computation.

10.4. **A registered rating is a snapshot.** A registered Gauntlet Verdict states the rating at its As-Of Time. It MUST NOT be presented as current after its Subject Record becomes Historical, after the Rubric version it used is superseded, or after Reproof Status changes.

10.5. **Authorship Disclosure.** 077 21.15 applies to this specification and to every Rater, Ranker, and Renderer implementing it.

## 11. Humans and Machines

11.1. 077 21.14 applies to every function of this specification.

11.2. **Human rendering.** A human rendering of a Gauntlet Verdict MUST show, in this order: where the proof is found (3.5); the Gate Verdict and each check; and, only if the Gate passed, the Strength Rating and Band, each Dimension's Level with a one-line reason drawn from the record, the Ceilings applied, the efficacy metrics of 7.7, the reproof history of 7.8, Current or Historical, the Rubric version, and the As-Of Time. It MUST be producible from the machine form alone.

11.3. **Labels.** A rendering MUST NOT use "certified", "approved", "compliant", "verified product", or similar terms for any Gate Verdict, Strength Rating, Band, or rank. The words are `valid`, `valid_legacy`, `invalid`, `unverifiable`, and the Band names.

## 12. Integration Points

12.1. This specification adds:

| ID | Integration Point | What plugs in | MUST do | MUST emit |
|---|---|---|---|---|
| **IP-G1** | Record Resolver | Registry clients | Resolve a Locator from a Source Registry meeting Section 4 through its public API only | Subject Record, receipt, envelope, Registry Profile status |
| **IP-G2** | Gate Checker | Verifiers, agents, browser clients | Apply Section 6 exactly; identical verdicts to the conformance vectors | Gate Verdict with evidence |
| **IP-G3** | Rater | Rating engines, browser clients | Extract Rating Inputs (7.2) and apply the named Rubric exactly; identical output to the conformance vectors | Gauntlet Verdict |
| **IP-G4** | Ranker | Ranking services, dashboards | Apply Section 9 exactly from Gauntlet Verdicts | Ranking Set, Ranking Snapshot |
| **IP-G5** | Renderer | Websites, reports, agent tools | Render under Section 11 from the machine form alone | Human rendering |

12.2. **Separation.** A Ranker MUST NOT accept anything of value for inclusion, position, or exclusion (9.5). A Rater that is a party to the Subject Record's Party Set MUST disclose it (10.3).

12.3. **Conformance vectors.** [vectors/gauntlet-vectors-0.1.0.json](vectors/gauntlet-vectors-0.1.0.json) gives Rating Inputs and the Levels, Ceilings, Strength Rating, and Band each MUST produce under `PGR-0.1.0`. An IP-G3 implementation that disagrees with any vector is non-conforming.

## 13. Security Considerations

- **Cherry-picking:** A Provider cites only its strongest record. Mitigated by mandatory inclusion of every public record (9.2) and Entries that show the rest beside the best (9.3).
- **Rating shopping:** A Provider looks for a Rater that scores it higher. Mitigated by determinism (7.1) and conformance vectors (12.3): every conforming Rater gives the same answer.
- **Goodhart pressure:** Providers optimize records for the Rubric rather than for truth. Every Dimension reads a property that also makes a proof harder to fake (independence, capture at the boundary, reproof, freshness), so optimizing for the Rubric is optimizing for stronger proof. Efficacy metrics are shown beside strength (7.7) so a strong record of a weak control stays visible.
- **Stale strength:** A rating earned before a change is shown after it. Mitigated by Freshness, the `historical` input, and exclusion of Historical ratings from rankings (9.2).
- **Technique padding:** Many techniques, each with one canary. Mitigated by `min_uop_per_technique` and by counting only adjudicated bad canaries in valid runs.
- **Legacy laundering:** A thin record presented as strong because it is "registered". Mitigated by `valid_legacy`, Ceiling K3, and Level 0 for every Dimension a Legacy Record cannot establish.
- **Key substitution:** A forged record carries its own key. Mitigated by resolving keys from a source other than the record (G5, 4.5).
- **Registry outage as attack:** An unreachable registry is used to freeze an old `valid`. Mitigated by 5.6 and 6.4: unreachable is `unverifiable`, never a cached pass.
- **Bot walls:** A registry or Gauntlet Service that blocks agents cannot be relied on by agents. Mitigated by 4.2 and 5.4.
- **Rubric capture:** A funder or Provider pushes thresholds in its favor. Mitigated by immutable, versioned Rubrics (7.9), changes recorded with reasons, funding disclosure (9.5), and the Proof Economy Code of Ethics.
- **Upload harvesting:** A rating service collects confidential records submitted for rating. Mitigated by stateless endpoints (5.5) and local computation.
- **Sybil reproof:** Several nominally independent checkers inflate Independence. Mitigated by 081 9.3, which counts only mutually independent Check Party Sets.

## 14. Reserved

Nothing in this specification assigns value to any record or to any Unit of Proof. Additional Dimensions are reserved for later Rubric versions and are added only by a new Rubric version under 7.9.

## 15. Interpretation

Craig Thomas Ellrod, as maintainer of Proof Protocol, is responsible for the editorial text of this specification and its Rubrics. Whether a Gate Verdict, Strength Rating, or Ranking Set conforms is determined from the records, the Rubric, and the normative text, and is not decided by the maintainer.

## Appendix A. Worked Examples (informative)

All four are computed under `PGR-0.1.0` and appear as conformance vectors.

**A.1. Valid, strong (88).** Full Record. Email security DUT. Gold claims tested, 34% mutated and holdout variants (D1 = 4). Eight qualifying techniques (D2 = 3). `lab-replica` from an Implementor Parity Declaration with no dimension `absent` (D3 = 3). Coverage published, Blind plus Void 4%, calibration current, ingress and fingerprints captured by an independent party (D4 = 4). Independent parties, Reproof Status `reproven (instance, 1)` (D5 = 3). Six days old (D6 = 4).
`25 + 15 + 11.25 + 15 + 11.25 + 10 = 87.5`, rounded half up: **88**, `strong`. Ceilings: none.

**A.2. Valid, weak (31).** Full Record. EDR DUT. Efficacy Rate with False Block Rate, no Gold canaries (D1 = 2). One qualifying technique (D2 = 1). `dev` (D3 = 1). Witness record, no coverage map (D4 = 1). Issuer is also the Proof Witness (D5 = 1). Ten months old (D6 = 1).
`12.5 + 5 + 3.75 + 3.75 + 3.75 + 2.5 = 31.25`: **31**, `weak`. Ceilings: none.

**A.3. Invalid.** Full Record. WAF DUT. G1 to G6 pass. G7 fails: the commitment's Time Anchor pulse is four minutes after the first ingress record. Gate Verdict `invalid`. No score, no band, no Levels (6.3).

**A.4. Legacy record from proofregister.com.** A PP-SPEC-004 ProofRecord, `status: active`, root hash verifies, NIST pulse matches, anchored twenty days before the As-Of Time. Gate Verdict `valid_legacy`. D1 to D5 are Level 0, `not_in_record`; D6 = 4.
`0 + 0 + 0 + 0 + 0 + 10 = 10`: **10**, `weak`. Ceilings K1 and K3 apply (neither lowers it). The rendering tells the reader exactly which fields would have to be registered, under 077 Section 21, for each Dimension to rise.

## Revision History

| Version | Date | Changes |
|---|---|---|
| 0.1.0 | 2026-11-02 | Initial draft. Any record listed in a PP-SPEC-004 registry, starting with proofregister.com, as Subject Record; Full and Legacy Records. Registry Profile, API first for registries and for the Gauntlet Service. The Gate (G1 to G9, four verdicts). The Strength Rating: six Dimensions, Levels 0 to 4, Weights, Ceilings K1 to K3, Bands; Rubric `PGR-0.1.0`. Rankings with mandatory inclusion and selection disclosure. Gauntlet Verdict Record and Ranking Snapshot, tag `PSP-1:record`. IP-G1 to IP-G5. Worked examples and conformance vectors. |
