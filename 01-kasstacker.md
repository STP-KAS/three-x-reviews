# Independent review 1 — KasStacker

**Post:** [ReconProtocol quoting GoonBoyCrypto, 14 Sep 2026](https://x.com/reconprotocol/status/2099513163112652929)  
**Quoted:** [GoonBoyCrypto launch thread](https://x.com/GoonBoyCrypto/status/2099497677100601394)  
**Live:** [kasstacker.org](https://kasstacker.org)  
**Starter:** [ItsGoonBoyCrypto/kasstacker-starter](https://github.com/ItsGoonBoyCrypto/kasstacker-starter)  
**This is:** a claim-and-product review. Not an audit. Not a listing.

---

## What I did

1. Read the quote-tweet and the full launch thread, including the six numbered claims and the credit list.
2. Opened kasstacker.org and walked home, `/status`, `/ecosystem`, `/compare`, `/learn`, `/learn/try-it`, `/stack/vprogs`, `/stack/kcc-20`, `/stack/silverscript`.
3. Hit the indexer the site says it uses: `https://kcc20.info/v1/stats` (15 Sep 2026).
4. Cross-checked the same quantities on kascov mainnet live + templates.
5. Looked up the site GitHub the ecosystem page names: `ItsGoonBoyCrypto/KasStacker` — **404**. Public user repos do include `kasstacker-starter`.
6. Checked the vProgs row against [kaspanet/vprogs](https://github.com/kaspanet/vprogs), which is public.

I did not compile `simple.sil` locally this pass. I did not replay the "thief" buttons against a live TN10 UTXO.

## Why I did it that way

The tweet is not a protocol. It is a documentation product claiming a standard: nothing unpublished unverified, dates on claims, "unverified" instead of guesses, daily CI on community code, no paid placement.

If those rules hold, the site is useful. If they do not, it is another Kaspa landing page with better copy. The chain numbers and the GitHub rows are the only things that can falsify it.

## What this actually is

A static education site for the post-Toccata stack. Three doors (learn / compare / stack), a daily-ish status board, an ecosystem directory, a six-stop tour, and a browser "break a covenant" lesson built on the twelve-line `SimpleCovenant`.

It is not a compiler, not an indexer, not a wallet, not a venue. kcc20.info and GitHub API are the live feeds. The site assembles them.

GoonBoyCrypto built DAGmate and is shipping this as "the missing manual." Recon's quote is an endorsement, not the product.

## What holds

**The status board is the right shape.** Component, maturity, repo, tag, last push, stars, issues. Silverscript **pre-audit** on v1.0.0 is the correct label. Argent **experimental** / no release is the correct label. "Fetched daily by a workflow that fails loudly" is the rule you want even if I cannot see that workflow.

**The under-review list is honest.** OpenSilver: 21 of 25 contracts no longer parse against current `silverc`. kosign, K100bet, kasmelt-snapshot same. That is more useful than a fake green directory.

**The try-it lesson teaches the real hole.** The contract only pins output 0's `scriptPubKey`. Amounts, extra outputs, and who signs are free. The page says so. The starter README says so. That is how you teach covenants without lying that twelve lines are a vault.

**kcc20.info numbers match the site's yesterday snapshot.** Status (fetched 2026-09-14): 162,527 created / 54,013 transitions / 161,847 burns / 120 live of 312 KCC-20. Live kcc20.info on 15 Sep: genesis 162,578 / transition 54,187 / burn 161,891 / KCC20 live 120 of 312. The daily lag is real, not invented.

**No wallet connect, no token, no paid placement copy.** The listing rule ("public code or on-chain proof") is the right bar. Enclave is labelled "source not yet public." Warda is labelled testnet and unaudited by its own statement.

**kasstacker-starter is a real first compile.** Clone silverscript, `./compile.sh`, get `artifact.json`. Daily CI on that public template is the part of "commands re-proven daily" I can actually see.

## Opinion

This is the education layer Kaspa should have shipped the week Toccata activated. Plain language first is the correct order. Maturity badges are the correct discipline. The thief lesson is the correct pedagogy.

Then the homepage talks like a press release and the vProgs page pretends a public kaspanet repo does not exist. A site whose brand is "unknown things say unverified" does not get to miss `kaspanet/vprogs`.

Do not add more ecosystem cards next. Publish the site repo. Fix the live-vs-all-time number. Put vProgs on the board as public / prototype. Drop KCC-20 from **production** until there is a tagged spec.

## Result analysis

| Claim in the tweets / site | What I measured 15 Sep 2026 |
| --- | --- |
| 160,000+ / 162,521 / 162,527 covenants "enforcing rules on mainnet" | **All-time genesis**, not live. kcc20.info: 162,578 genesis, 161,891 burns → ~687 still alive. kascov: 162,578 covenants, **730 active**, 161,848 burned. |
| 53,979 / 54,013 state transitions | kcc20.info **54,187**. Same series, one day later. Holds. |
| "Live from the repos — never hand-written" | Status refresh `2026-09-14 15:18 UTC`. Plausible. Cannot see the workflow: site repo **404**. |
| KCC-20 **production** | Live tokens **120 / 312** on kcc20.info. Repo `kaspanet/kccs` **no release**. Site itself: "written standard is still being consolidated" and token support **unverified**. Production is the wrong badge. |
| vProgs "none public / unconfirmed", "No public links verified yet", "nothing to build with yet" | [kaspanet/vprogs](https://github.com/kaspanet/vprogs) is public, 79 commits, last push ~10 Sep 2026, README: early development / prototype. The "unverified" rule required listing it. |
| "You attack a real, compiling contract" / "watch the network reject" | `/learn/try-it` is a **browser lesson** on `SimpleCovenant`. It is not a TN10 broadcast I could see. Compiling is real in kasstacker-starter. Network reject is theatre unless they wire it to a node. |
| "Community code compile-checked daily in CI" | Starter CI is public. The named workflow `ItsGoonBoyCrypto/KasStacker/actions/workflows/verify-community-examples.yml` is **404**. |
| "Every one verified before it's shown" | Speed: several cards stamped "reviewed 2026-09-14" the launch day. Verification depth is not inspectable without the private repo. |
| Origins labelled, no paid placement | Copy holds. I cannot audit the inbox. |

Mainnet covenant mix (kascov templates) is the number the homepage should have used:

| Template | ever | live |
| --- | --- | --- |
| KasWare vault-pqcold | 73,150 | 0 |
| KasWare vault-schnorr | 73,117 | 0 |
| KasWare vault-htlc | 12,939 | 0 |
| KasWare vault-dms | 795 | 0 |
| KCC20 token | 41,953 states / 115 coins | 3,084 states |
| p2sh commitment | 34,573 | 703 |
| SilverScript Escrow | 4 | 0 |

Almost the entire 162k is **finished KasWare vault churn**, not 162k programs currently enforcing anything. Burns ≈ genesis. That is not a scandal. It is the honest picture. The homepage hid it.

kascov vs kcc20.info disagree on KCC-20 "live" (3,084 states vs 120 tokens). KasStacker picked the indexer it named. Fine. It should say which unit.

## Breaking points

1. **Homepage vs status page.** Status: "Covenants created, genesis events, all time." Homepage: "162,527 covenants enforcing rules on Kaspa mainnet." Those are different sentences. The second is false.
2. **vProgs miss.** A documentation site that claims it will show unknown rather than guess has guessed "no public repo" against `kaspanet/vprogs`.
3. **KCC-20 production with no tag and an unfinished spec.** Compare matrix even says token support unverified. Pick one.
4. **Site source is private.** The product that lectures "origins always labelled" and "CI fails loudly" does not let you read the CI. `kasstacker-starter` is not a substitute for the site.
5. **"Network reject" demo.** If the thief button does not submit to a node, do not say "watch the network reject your steal like a coin that doesn't fit the slot." Say "watch why the script would fail." The lesson is still good.
6. **Launch-day reviews.** KasProof, Kassword, KasRanks, Enclave all "reviewed 2026-09-14." Either the bar is a checklist or the word "reviewed" is doing too much work. Show the checklist.
7. **Indexer as single source.** kcc20.info is keyless and good. It is still one indexer. kascov already publishes `/data/mainnet/consistency.json` for disagreements. KasStacker should surface that, not only kcc20.info.

## How to improve

- Change the homepage counter to "created, all time" and add "alive now" from the same feed (~700, not 162k).
- Add a KasWare vs KCC-20 vs unrevealed-P2SH breakdown. The 162k is otherwise a vanity number.
- Put `kaspanet/vprogs` on the board: maturity **roadmap/prototype**, repo public, "not a covenant you can deploy today."
- Move KCC-20 to **draft / consolidating** until a tagged KCC. Keep "on mainnet: yes."
- Make `ItsGoonBoyCrypto/KasStacker` public, or publish the verify workflow and the listing checklist as files in kasstacker-starter.
- Wire try-it to a documented TN10 reject (txid or node error), or drop the word "network."
- Date every ecosystem "verified" with what was checked: compile, on-chain template hash, or homepage visit.

## Send this

This issue. Not the other two. KasStacker is a docs product. x402 and kascov are different jobs.
