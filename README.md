# ProofGauntlet™: The Proof Strength Protocol

> **Ownership and Attribution Notice**
>
> ProofGauntlet and the Proof Strength Protocol (PP-SPEC-100) are authored by **Craig Thomas Ellrod** and published as part of **Proof Protocol**. Copyright (c) 2026 Craig Thomas Ellrod.
>
> Any use, implementation, derivative, citation, or incorporation of this work, in whole or in part, MUST attribute **Craig Thomas Ellrod and Proof Protocol**. See [NOTICE.md](NOTICE.md).
>
> Licensed under the Proof Protocol License 1.1 (see LICENSE.md); text CC BY 4.0 under Part A, which grants no patent or trademark rights. Original inventor: Craig Thomas Ellrod (ORCID 0009-0004-1058-7666).
>
> **Patents.** Patent terms are in [PATENT-SCHEDULE.md](PATENT-SCHEDULE.md). Claims not listed there are reserved by Craig Thomas Ellrod. Licensing: 1proofprotocol@gmail.com.
>
> **Trademarks.** ProofGauntlet™ and Proof Gauntlet™ are trademarks of Craig Thomas Ellrod.

---

**Valid is the floor. Strength is the score.**

A registry tells you a proof exists and has not been changed. It does not tell you how much the proof can carry. ProofGauntlet does: any record listed in a PP-SPEC-004 registry, starting with [proofregister.com](https://proofregister.com), goes through the Gate, and if it passes, gets a Strength Rating from 0 to 100 under a published rubric that anyone can recompute.

## How it works

```
 Any record listed in a PP-SPEC-004 registry  (proofregister.com first)
        |
        |  resolved through the registry's public API, by Locator
        v
 THE GATE  (G1 to G9)
   registered, not revoked, intact, anchored, signed,
   schema, committed before the run, valid run, Conformant
        |
   +----+--------------+----------------+------------------+
   |                   |                |                  |
 invalid          unverifiable        valid          valid_legacy
 no score         no score              |            (004 fields only)
 shows why        shows why             |                  |
                                        v                  v
                         THE STRENGTH RATING  (Rubric PGR-0.1.0)
                           D1 Tier                    25
                           D2 Adversarial Depth       20
                           D3 Environment Fidelity    15
                           D4 Capture Fidelity        15
                           D5 Independence            15
                           D6 Freshness               10
                           Levels 0 to 4, Ceilings K1 to K3
                                        |
                                        v
                  0 to 100:  weak | moderate | strong | extremely_strong
                  always shown with where it is found, efficacy beside it,
                  reproof history, rubric version, as-of time
                                        |
                                        v
                  RANKINGS: every public record, best per DUT,
                  with the rest shown beside it
```

1. **Start with any proof.** Every record a conforming registry lists can be gated and rated. No one has to submit it, and no Provider can opt out.
2. **Gate first.** Fail any check and there is no score, no band, no rank. Unknown is `unverifiable`, never `valid`.
3. **Rate by rubric, not opinion.** Six Dimensions, Levels decided by fields the record already carries. Same record, same rubric, same time: same answer, for a person or a machine.
4. **Unknown is zero.** A Dimension the record cannot establish scores nothing. Thin records score low. Honestly.
5. **Strength is not efficacy.** A strong proof of a weak control is still a strong proof. Efficacy and False Block rates are always shown beside the rating.
6. **Rank everything.** Rankings include every public record and show the rest beside the best, so cherry-picking is visible.

## API first

The API is the product. The website is a client of it.

| Method | Path | |
|---|---|---|
| GET | `/registries` | Source Registries and their Registry Profile status |
| GET | `/gate/{registry_id}/{record_id}` | Gate Verdict for a listed record |
| POST | `/gate` | Gate Verdict for a supplied record (stateless) |
| GET | `/rating/{registry_id}/{record_id}` | Gauntlet Verdict: Gate plus Strength Rating |
| POST | `/rating` | Gauntlet Verdict for a supplied record or Rating Inputs (stateless) |
| GET | `/rankings` | Ranking Set for a declared filter |
| GET | `/rubrics`, `/rubrics/{version}` | Published rubrics |
| GET | `/ratings/{id}` | Registered Gauntlet Verdict Records and Ranking Snapshots |

Base: `https://proofgauntlet.com/api/v1`. No authentication for reads. No bot walls. CORS open. Nothing you POST is stored unless you ask. And every registry ProofGauntlet reads from must meet the same rules (SPEC Section 4).

```
curl https://proofgauntlet.com/api/v1/rating/proofregister.com/PR-2026-00001
```

## Repository

| Path | Contents |
|---|---|
| [SPEC.md](SPEC.md) | The normative specification, PP-SPEC-100 |
| [rubric/PGR-0.1.0.json](rubric/PGR-0.1.0.json) | The rubric: Levels, thresholds, Weights, Ceilings, Bands |
| [openapi/proofgauntlet.openapi.yaml](openapi/proofgauntlet.openapi.yaml) | The API, OpenAPI 3.1 |
| [schemas/gauntlet.schema.json](schemas/gauntlet.schema.json) | Gate Verdict, Rating Inputs, Gauntlet Verdict, Ranking Set, Ranking Snapshot |
| [schemas/ai-disclosure.schema.json](schemas/ai-disclosure.schema.json) | Authorship Disclosure schema (077 21.15) |
| [vectors/gauntlet-vectors-0.1.0.json](vectors/gauntlet-vectors-0.1.0.json) | Conformance vectors |
| [docs/overview.md](docs/overview.md) | Plain-English walkthrough |
| [docs/api.md](docs/api.md) | API guide, for people and agents |
| [docs/glossary.md](docs/glossary.md) | Terms |
| [docs/conformance.md](docs/conformance.md) | Conformance checklist |
| [docs/faq.md](docs/faq.md) | Common questions |
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to engage |
| [NOTICE.md](NOTICE.md) | Attribution requirements |
| [AI_DISCLOSURE.md](AI_DISCLOSURE.md) | What was human-authored and what was AI-drafted |
| [FUNDING.md](FUNDING.md) | Funding disclosure |
| [REVIEWERS.md](REVIEWERS.md) | Review disclosure |
| [ACKNOWLEDGEMENTS.md](ACKNOWLEDGEMENTS.md) | Credits |
| [ETHICS.md](ETHICS.md) | Proof Economy Code of Ethics |
| [CITATION.cff](CITATION.cff) | Citation metadata |
| [SECURITY.md](SECURITY.md) | Reporting protocol flaws |
| [PATENT-SCHEDULE.md](PATENT-SCHEDULE.md) | Patent terms |
| [LICENSE.md](LICENSE.md) | License terms |
| [LICENSE-CC-BY-4.0.md](LICENSE-CC-BY-4.0.md) | CC BY 4.0, specification text |
| [LICENSE-APACHE-2.0.md](LICENSE-APACHE-2.0.md) | Apache 2.0, for reference (A6); no version of this specification was released under it |

## Status

Draft, version 0.1.0, 2026-11-02. Maintained by Craig Thomas Ellrod as part of Proof Protocol.

Reads records registered under [PP-SPEC-004](https://github.com/proofprotocol/registry-api-spec); the public instance is [proofregister.com](https://proofregister.com). Builds on PP-SPEC-077 (Dead Canary), PP-SPEC-078 (Jailed Canary), PP-SPEC-079 (Coal Mine), PP-SPEC-080 (Pithead), and PP-SPEC-081 (Reproof). Public instance of this specification: [proofgauntlet.com](https://proofgauntlet.com).

Related: Proof Economy Prior Art Declaration, DOI [10.5281/zenodo.21379780](https://doi.org/10.5281/zenodo.21379780).

## Cite

```
Ellrod, C. (2026). ProofGauntlet: The Proof Strength Protocol (PP-SPEC-100), v0.1.0.
Craig Thomas Ellrod, Proof Protocol. https://github.com/proofprotocol/PP-SPEC-100-PROOFGAUNTLET
```
