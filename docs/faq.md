# FAQ

**Is a high Strength Rating a sign of a good product?**
No. It is a sign of a heavy proof. A strong proof can show a control missing half its attacks. That is why Efficacy Rate and False Block Rate are always shown beside it (SPEC 7.7), and why rankings are labeled as rankings of proofs, not products (9.6).

**Who decides the score?**
The rubric does. It is a published, immutable data file. Anyone can recompute any score. If two implementations disagree, one of them is wrong, and the conformance vectors say which (7.1, 12.3).

**Can a vendor keep a weak proof off ProofGauntlet?**
Not a public one. Every public record a conforming registry lists is gated, rated, and ranked, and a ranking shows a product's other records beside its best one (9.2, 9.3). A Holder controls disclosure level when the record is registered, not whether a public record is rated.

**What happens to records in proofregister.com that were registered before PP-SPEC-077 Section 21?**
They are Legacy Records. They go through the Gate checks a PP-SPEC-004 record can support and are rated on what they can show, which usually means only Freshness. They are not excluded, and the rendering says what to register to raise each Dimension (3.2, Appendix A.4).

**Why does an unknown count as zero?**
Because an unknown assumed favorable is a gift to whoever withheld it. A Dimension the record cannot establish scores nothing (7.3.7).

**Why can't ProofGauntlet read from a registry behind a bot check?**
Because agents are consumers like any other (077 21.14.5), and a proof only an approved browser can read is not a public proof. Registries must meet the Registry Profile (Section 4) for their records to be ranked.

**Does ProofGauntlet store the records I check?**
Not unless you ask. `POST /gate` and `POST /rating` are stateless by default, and any client can compute a rating locally from the published rubric without sending the record anywhere (5.5).

**Is a Strength Rating a value or a price?**
No. Nothing in this specification assigns value to any record or Unit of Proof (Section 14).

**Does a rating ever change?**
A rating is computed at an As-Of Time. As a record ages, is reproven, or diverges, a new computation gives a new rating. A registered Gauntlet Verdict is a snapshot of one moment and says so (10.4).

**How does this relate to PP-SPEC-080?**
PP-SPEC-080 decides whether a reader accepts a record. ProofGauntlet never does. An acceptance profile MAY require a minimum Strength Band.
