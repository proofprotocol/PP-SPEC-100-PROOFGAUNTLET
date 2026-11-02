# Conformance checklist

A checklist, not a substitute for SPEC.md. Each line cites its section.

## Source Registry (Registry Profile, Section 4)

- [ ] PP-SPEC-004 conformant: five endpoints, append-only, revocation as a flag, no authentication for queries (004 Section 9)
- [ ] Every website function is available through the public API, and the website uses only that API (4.1)
- [ ] No CAPTCHA, browser check, or JavaScript challenge on API paths; `robots.txt` does not exclude agents from the API (4.2)
- [ ] Rate limits stated in response headers (4.2)
- [ ] `GET /records` enumerates every record, including revoked, with a stable cursor and `since` (4.3)
- [ ] `GET /records/:id/verify` exposed; receipts and signature envelopes returned for Full Records (4.4)
- [ ] Receipt verification keys published somewhere other than API responses (4.5)
- [ ] CORS permits `GET` from any origin (4.6)

## Gauntlet Service (Section 5)

- [ ] Every function available through the API no later than through any interface (5.1)
- [ ] Versioned paths; earlier major version kept twelve months (5.2)
- [ ] All endpoints of 5.3; OpenAPI at `/openapi.json`
- [ ] No authentication on reads or on `POST /gate` and `POST /rating` (5.4)
- [ ] Request bodies not stored without `store=true`; local computation possible (5.5)
- [ ] Problem details for errors; unreachable registry gives `unverifiable` (5.6)
- [ ] Any MCP or agent interface is a client of the API and adds nothing (5.7)

## Gate Checker (IP-G2, Section 6)

- [ ] Applies G1 to G9 in order, with evidence per check (6.1)
- [ ] Verdict rules of 6.2 exactly
- [ ] No score, band, Level, or rank for `invalid` or `unverifiable` (6.3)
- [ ] Unperformable check is `unverifiable`; no stale cached pass for G1 or G2 (6.4)
- [ ] `invalid` public verdicts returned on request (6.5)

## Rater (IP-G3, Section 7)

- [ ] Uses only the Rating Inputs of 7.2, each with its source; null inputs carry a reason
- [ ] Levels, Weights, Ceilings, rounding, and Bands exactly as the named Rubric (7.3 to 7.6)
- [ ] Unknown inputs give Level 0 (7.3.7)
- [ ] Efficacy Rate, False Block Rate, Blind and Void shares shown beside every efficacy rating (7.7)
- [ ] Reproof scope, count, and history carried (7.8)
- [ ] Computes under every published Rubric version (7.9)
- [ ] Matches every conformance vector (12.3)

## Ranker (IP-G4, Section 9)

- [ ] Declared filter; no mixed Technology Profiles unless declared (9.1)
- [ ] Every matching public Current record included; no opt-out (9.2)
- [ ] Each Entry shows other rated records, their median, and invalid, unverifiable, revoked, and non-public counts (9.3)
- [ ] Ordering of 9.4 exactly
- [ ] Nothing of value changes inclusion or position; own funding disclosed (9.5)
- [ ] Labeled as a ranking of proofs, not products (9.6)

## Renderer (IP-G5, Section 11)

- [ ] Order of 11.2: where found, Gate, then rating details
- [ ] Producible from the machine form alone
- [ ] No "certified", "approved", "compliant", or similar labels (11.3)
- [ ] Any PP-SPEC-004 `pes_score` shown labeled by name, never merged (3.4)

## Every implementation

- [ ] Authorship Disclosure under 077 21.15 (10.5)
- [ ] Attribution under NOTICE.md
