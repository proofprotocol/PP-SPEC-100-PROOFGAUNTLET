# ProofGauntlet in plain English

## The problem

Proof Protocol records are sealed, signed, anchored, and registered. Anyone can check that a record is the one that was made. But two perfectly valid records can mean very different things. One is a single attack, fired once at a development box a year ago, judged by the same party that launched it. The other is a campaign across many techniques, run last week in an environment built to match the customer's, watched by an independent witness, and repeated by a second, unrelated party who got the same answer.

Both are "valid". Only one of them should be trusted with a purchase, a policy, or a premium.

## What ProofGauntlet does

Give it any proof listed in proofregister.com (or any registry that meets the rules in SPEC Section 4). It does two things.

**1. The Gate: is it real?** Nine checks: the record is registered, not revoked, unchanged, anchored to a public time beacon, signed by keys that come from somewhere other than the record itself, in the right format, committed before the attack ran, from a valid run, and Conformant. Fail one and the answer is `invalid`, with the reason. If a check cannot be done, the answer is `unverifiable`, never a guess. Either way there is no score.

**2. The Strength Rating: how much can it carry?** A record that passes gets a score from 0 to 100, built from six questions:

| Dimension | The question | Weight |
|---|---|---|
| Tier | Was the control shown to work, or only shown to exist? | 25 |
| Adversarial Depth | How many techniques, across how many tactics? | 20 |
| Environment Fidelity | How close was the test to the real environment? | 15 |
| Capture Fidelity | Was the evidence honest when it was captured? | 15 |
| Independence | Did anyone with a stake take part? Was it reproven? | 15 |
| Freshness | Is it still true today? | 10 |

Each question gets a Level from 0 to 4, decided by facts the record already contains. Anything the record cannot show scores zero. Some conditions cap the score: no proof of efficacy caps it at 40; a failed reproduction caps it at 50; an old-format record caps it at 40.

The score lands in a band: `weak` (0 to 39), `moderate` (40 to 69), `strong` (70 to 89), or `extremely_strong` (90 to 100).

## What makes it different

- **No opinion.** The rubric is a published data file. Anyone can recompute any score and get the same number.
- **No hiding.** Every public record is rated and ranked. No vendor can opt out, and a ranking always shows the rest of a product's records beside its best one.
- **Strength is not efficacy.** A strong proof can show a control failing. Efficacy and false block rates are always shown beside the strength.
- **API first.** Everything is an API call first. The website is just a client. The same rule applies to the registries it reads.
- **Nothing is changed.** ProofGauntlet never edits a record. It computes, like a calculator.

## Old records

Records registered before PP-SPEC-077 Section 21 carry only a product name, a hash, and a time-beacon reference. They are not thrown out. They go through the checks they can support and are rated on what they can show, which is usually only Freshness. The rendering says exactly what would have to be registered for each score to rise.
