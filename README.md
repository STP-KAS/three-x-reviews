# three-x-reviews

Independent reviews of **three X posts from 14 September 2026**, filed separately, in one GitHub.

Not Kaspa core. Not an audit. Not a token. Not a listing.

| | |
| --- | --- |
| When | 15 September 2026 |
| Desk | [STP-KAS](https://github.com/STP-KAS) |
| Style | same as [grok-test-cascade](https://github.com/STP-KAS/grok-test-cascade) and [tn10-hard-test](https://github.com/STP-KAS/tn10-hard-test): what I did, why, what it is, what holds, honest opinion, result analysis, breakpoints, how to improve |
| Testnet | used where the claim lives on TN10. No funded spend this pass. |

## The three, separately

| Review | Post | Product | File | Issue |
| --- | --- | --- | --- | --- |
| 1 | [@ReconProtocol quoting @GoonBoyCrypto](https://x.com/reconprotocol/status/2099513163112652929) | [kasstacker.org](https://kasstacker.org) | [01-kasstacker.md](01-kasstacker.md) | [#2](https://github.com/STP-KAS/three-x-reviews/issues/2) |
| 2 | [@ReconProtocol 1/10 x402 thread](https://x.com/reconprotocol/status/2099560794035736754) | [kaspa-x402.org](https://kaspa-x402.org) / [elldeeone/kaspa-x402](https://github.com/elldeeone/kaspa-x402) | [02-kaspa-x402.md](02-kaspa-x402.md) | [#1](https://github.com/STP-KAS/three-x-reviews/issues/1) |
| 3 | [@0xKnitser quoting @kascovio](https://x.com/0xknitser/status/2099587647018873308) | [kascov.io](https://kascov.io) | [03-kascov.md](03-kascov.md) | [#3](https://github.com/STP-KAS/three-x-reviews/issues/3) |

GitHub issue numbers are creation order, not review order. Send the issue, not the review number.

Read the files. The issues are the same text so they can be sent one at a time.

## One-line verdicts

| Target | Verdict |
| --- | --- |
| **KasStacker** | Best education layer the stack has. The honesty rules are real and then the homepage breaks them. vProgs is public and they say it is not. The site repo is private. |
| **kaspa-x402 (Recon thread)** | Luke's binding is the most serious Kaspa payment work on TN10. Recon's thread sells it one tense too early: not joined, not CAIP, not production SilverScript, not "one second" for the covenant path. |
| **kascov artifact** | The feature does what it says: garbage artifacts are refused. The explorer is the real public good. The verifier going private is the integrity hole. |

## What this pass actually hit

Live HTTP/JSON on 15 Sep 2026:

- kasstacker.org (home, status, ecosystem, compare, learn, try-it, vProgs, KCC-20)
- kcc20.info `/v1/stats`
- kascov.io live feeds, templates, artifact POST, openapi
- demo.kaspa-x402.org health / supported / canary / unpaid `402`
- kaspa-x402.org release.json + mainnet-readiness
- api.kaspa.org hashrate
- GitHub: `ItsGoonBoyCrypto/KasStacker` **404**, `kaspanet/vprogs` public, `elldeeone/kaspa-x402`, `Knitser/kascov` README-only

Not done this pass: a funded groks-wallet x402 settle, a hash-mismatch argentc `artifact.json` (schema refuse was proven; hash-reproduce refuse was not), counting 388/65 named builds one-by-one.

Sources: [SOURCES.md](SOURCES.md).

## Why three issues, one repo

Same pattern as tn10-hard-test. Upstream trackers are often 403 for this account. One GitHub, three independent write-ups, no mash-up of KasStacker vs x402 vs kascov.

If you only send one link: send the matching issue, not this README.
