# Independent review 2 — Recon x402 thread / kaspa-x402

**Post:** [ReconProtocol 1/10, 14 Sep 2026](https://x.com/reconprotocol/status/2099560794035736754)  
**Builder:** [@elldeeone](https://github.com/elldeeone/kaspa-x402) (Luke)  
**Live spec:** [kaspa-x402.org](https://kaspa-x402.org)  
**TN10 gateway:** [demo.kaspa-x402.org](https://demo.kaspa-x402.org)  
**This is:** a review of the **thread's claims** against Luke's own docs and a live TN10 probe. Not a re-audit of every package. Not mainnet advice.

This desk already wrote longer kaspa-x402 notes in [sixpack.wtf](https://github.com/STP-KAS/sixpack.wtf), [x402-vs-grok](https://github.com/STP-KAS/x402-vs-grok), [grok-heavy-test](https://github.com/STP-KAS/grok-heavy-test), and [tn10-hard-test#2](https://github.com/STP-KAS/tn10-hard-test/issues/2). This file is independent of those as a **claim check of this tweet**. Where they agree, that is because the code did not change.

---

## What I did

1. Read the full 1/10–10/10 thread, including the quoted stack diagram.
2. Read Luke's README, kaspa-x402.org status block, and [mainnet readiness](https://kaspa-x402.org/docs/mainnet-readiness/).
3. Hit the hosted gateway on 15 Sep 2026:
   - `GET /` service index
   - `GET /health` → `1.0.0-rc.1`, `kaspa-x402-testnet`, exact `standard-native`
   - `GET /supported` → x402 v2 `exact` + `batch-settlement` on `kaspa:testnet-10` only
   - `GET /canary` → scheduled checks **ok**; **paid exact canary skipped** (no spending keys on the scheduler)
   - unpaid `GET /exact/report` → HTTP **402** with `PAYMENT-REQUIRED` header
4. Decoded that 402 offer.
5. Cross-checked "already supported: Solana, EVM, TON, Stellar, Aptos, Hedera" against current [x402 network docs](https://docs.x402.org/core-concepts/network-and-token-support).
6. Looked at kascov TN10 template `x402 · escrow v2` (on-chain batch bodies exist).
7. Did **not** send a funded groks-wallet payment this pass.

## Why I did it that way

Recon is not the implementer. Luke is. The thread is the public story allocators will repeat. The honest document is already on kaspa-x402.org. If the thread and the spec disagree, the spec wins and the thread is marketing.

TN10 is the only network the release allows. "tn if needed" means probe the gateway, not pretend mainnet.

## What this actually is

A **proposed** x402 v2 native-Kaspa binding, release **`1.0.0-rc.1`**, network **`kaspa:testnet-10` only**.

Two schemes, both live on the hosted worker:

| Scheme | What it is | On-chain |
| --- | --- | --- |
| `exact` / `kaspa-exact-v2` | one native KAS transfer per request | ordinary UTXO; offer amount **20,000,000 sompi** (0.2 tKAS) on `/exact/report` |
| `batch-settlement` / `kaspa-escrow-v3` + `kaspa-x402-escrow-v4` | fund a SilverScript covenant once, sign vouchers, claim/refund later | kascov names `x402 · escrow v2` on TN10: 45 covenants, 25 live, 69 revealed runs |

Luke's own status line, generated from commit `6e8afc4ed918` (14 Sep): mainnet **blocked**; `kaspa:*` identifiers are **draft**, not accepted x402 registry or CAIP entries.

That is the product. The thread is a press pass of that product.

## What holds

**The gateway is real.** Health ok. Supported advertises both schemes on `kaspa:testnet-10`. Unpaid exact returns a proper x402 v2 402, not an HTML error page.

**Decoded unpaid exact offer (15 Sep):**

- `scheme`: `exact`
- `network`: `kaspa:testnet-10`
- `asset`: `KAS`
- `amount`: `20000000` sompi
- `binding`: `kaspa-exact-v2`
- `profile`: `standard-native`
- `finality`: **`accepted`**
- `payTo`: `kaspatest:qzlws9lm7uyt0tftzffshnyeu2zcqk4kf7hw5ghk6v0zh093vnkljcy2fl0fh`
- `maxTimeoutSeconds`: 300

**Batch bodies exist on TN10.** kascov is naming the escrow template from revealed bytes. This is not a whitepaper.

**The two-scheme design is the right Kaspa shape.** Exact for one-shot. Batch for dust-priced repeats, because KIP-9 will not let you settle 500 sompi as an output. Luke documents the 10,000,000 sompi gateway output policy as **policy**, not consensus dust. That sentence is worth more than the thread.

**Mainnet gates are written down.** Independent audit, consensus cross-validation of tx v1 shapes, independent chain evidence, durable store, distributed admission, recovery, live 18-flow proof. Recon's "each layer ships when it's ready" is Luke's actual policy. The thread then talks as if the payment protocol were already ready.

**x402 Foundation / Linux Foundation / Coinbase contribution** is a real 14 Jul 2026 launch. Cloudflare is a premier member. Recon's institutional frame is not invented.

**Kaspa as first PoW rail in that set is probably true.** Official x402 network docs list EVM, Solana, TON, Algorand, Stellar, Aptos, Hedera, Keeta, NEAR, Concordium, XRPL, Cardano. No Bitcoin. No Kaspa.

## Opinion

Luke built a binding. Recon sold a joining.

Read kaspa-x402.org and you get a testnet RC with blocked mainnet and draft identifiers. Read the thread and you get "Kaspa is joining the global payment standard for the internet" and "the payment protocol is ready."

Those are not the same sentence. The first is a PR. The second is a release candidate on TN10.

Do not upstream-submit until CAIP-2 and the x402 registry actually have a `kaspa` row. Do not call SilverScript batch "production." Do not tell people confirmation is ~1s if batch still waits 30 selected-chain confirmations. The work is good enough that it does not need those adjectives.

## Result analysis

| Thread claim | Result |
| --- | --- |
| "Kaspa is joining the global payment standard" | **Tense is wrong.** Proposed binding, RC1, not in the x402 network table, not a CAIP. |
| "This week, a kaspa:native binding is ready for upstream submission" | **Half.** Code+spec+gateway are in a state you *can* PR. Luke still says identifiers are draft / not accepted. "Ready to submit" ≠ "submitted" ≠ "accepted." |
| "Every chain currently in x402 is account based. Kaspa would be the first PoW and the first UTXO at internet speed." | **PoW: likely.** **UTXO: no, unless you hide Cardano.** Cardano (eUTXO) is already in the official x402 network list. XRPL is also listed (not classic UTXO). "Internet speed" is the hedge, and it is doing a lot of work. |
| "Live gateway verifies accepted Kaspa payments roughly one second after broadcast." | **Exact path: `finality: accepted`.** That is "accepted," not one second as a measured SLA, and not the batch path. Luke's README: TN10 **30 selected-chain confirmations** for each **covenant transition**. At 10 BPS that is ~3s of blocks plus whatever the confirmer actually waits. Recon described the happy exact advert, not the escrow. |
| Two schemes: exact + batch SilverScript | **Holds.** `/supported` and the 402 header match. Template id `kaspa-x402-escrow-v4`. |
| "SilverScript's first real world production use case" | **False twice.** It is **testnet**. KaspaRocket / KRON / Zealous / KForge / KaspaKaha already ship covenant/SilverScript venues (KasStacker's own ecosystem page lists them). x402 escrow is a serious use. It is not the first and it is not production. |
| "2.5 months of community review… alpha.9 is current" | **Stale inside the same week.** Live release is **`1.0.0-rc.1`**, not alpha.9. If alpha.9 was yesterday's build, the thread should have said RC1. |
| "Live testnet gateway has settled real testnet-10 payments" | **Luke claims a funded 18-flow + exact canary.** This pass: scheduled canary **skips** paid exact (no keys). I did not re-spend. Treat as *his* evidence, still consistent with a working worker. |
| Step 1: register `kaspa:mainnet` and `kaspa:testnet-10` at CASA | **Correct bottleneck, sloppy names.** CAIP-2 allows namespace `kaspa` (5 chars) and reference `testnet-10`. CASA/Chain Agnostic is the registry. Luke: not accepted yet. Recon calling the backlog the bottleneck is fair. Shipping `kaspa:mainnet` as an identifier while mainnet is blocked is how people will try mainnet too early. |
| Step 2: propose to x402 Foundation like Stellar/Aptos/Hedera | **The right door.** Those chains are in the docs. Kaspa is not. |
| "A server can list Ethereum-USDC, Solana-USDC, Kaspa-KAS in one 402" | **Protocol-true, ops-false today.** x402 v2 can carry multiple accepts. No production facilitator in the official set speaks `kaspa:*`. You need Luke's worker or a self-hosted facilitator. |
| "AI agents that don't hold KAS can use a facilitator" | **Specified.** Facilitator is optional in the Kaspa binding; the hosted demo *is* a facilitator-shaped gateway. Not a Coinbase CDP product. |
| 382 miners, 339.8 PH/s, 547,355 addresses ≥1 KAS, 162,521 covenants | **Hashrate ballpark:** api.kaspa.org `330841` on 15 Sep ≈ **330.8 PH/s** if the unit is GH/s. Tweet was 339.8 the day before. Miner count and holder count **not independently verified** this pass. Covenants: see review 1 — all-time genesis, not live. |
| Stack diagram: kaspa-x402 = rc.1, testnet | **This square of tweet 10/10 is honest.** It contradicts tweet 1/10. Believe 10/10. |

## Breaking points

1. **Present tense ("is joining", "is ready") on a draft CAIP and a blocked mainnet.** Allocators will quote tweet 1, not the readiness doc.
2. **`finality: accepted` vs 30-confirm batch.** Mix those and you get a merchant that serves on accept and an escrow that is not done. Document which scheme the "one second" line refers to, or delete it.
3. **"First UTXO chain" vs Cardano already in x402.** Fact-check the competitive sentence before the Foundation PR.
4. **"First production SilverScript" vs the rest of the ecosystem.** This will annoy every venue already compiling `silverc`. It is also how you get a language labelled pre-audit treated as production because a payment demo used it.
5. **alpha.9 vs rc.1.** Version drift in a 10-tweet launch is sloppy. The npm tag is `1.0.0-rc.1`.
6. **Hosted gateway is Cloudflare Worker + REST.** Fine for TN10. Luke's own mainnet gate says one RPC/provider is not enough. The live demo does not close that gate. Do not demo-as-mainnet.
7. **KIP-9.** 0.2 tKAS exact is above the 0.1 KAS output policy. Batch vouchers can be 500 sompi (canary batch-offer amount). If someone copies the canary price as an on-chain exact output, construction fails. The thread never says this. Luke's README does.
8. **No funded settle in this pass.** I can break the *copy*. I cannot break the *claim path* without keys. Scheduled canary also skipped paid exact. Anyone repeating "working code" should run `proof:live` themselves.

## How to improve

For **Luke** (the binding):

- Keep the kaspa-x402.org status block as the canonical tense. Ask quote-tweets to link it.
- In `/supported` and 402 extras, put `confirmations` / `finality` next to each scheme so a client does not assume exact's `accepted` for batch.
- File the CAIP-2 namespace PR with `kaspa:testnet-10` first. Do not lead with `kaspa:mainnet`.
- Rename "production use case" to "first x402 escrow on TN10" if you must market SilverScript.

For **Recon** (the thread):

- Tweet 1 should have been tweet 10. "RC1 on TN10, identifiers draft, mainnet blocked, CASA is the queue."
- Drop "first UTXO" or say "first high-BPS PoW UTXO."
- Drop "first production SilverScript."
- Publish the miner/holder queries. 382 and 547,355 without a source is newsletter math.
- When you cite 162k covenants, say all-time genesis. Review 1 is why.

## TN10 this pass

Used. Gateway live. 402 live. Paid path **not** re-executed. Canary: paid exact **skipped**. On-chain escrow template **visible** via kascov, not via a new spend from this desk.

## Send this

This issue. KasStacker and kascov are separate. Luke already has a Windows clone fix merged (`216ad77`). This is the Recon-thread overclaim, not that bug.
