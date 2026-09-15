# Independent review 3 — kascov "bring your own build"

**Post:** [0xKnitser quoting kascovio, 14 Sep 2026](https://x.com/0xknitser/status/2099587647018873308)  
**Quoted:** [kascovio "Bring your own build"](https://x.com/kascovio/status/2099586479739813947)  
**Live:** [kascov.io](https://kascov.io)  
**GitHub:** [Knitser/kascov](https://github.com/Knitser/kascov) — README only; app source private  
**This is:** a review of the new artifact-verify direction. Not a full explorer audit. Not a trading-terminal review.

---

## What I did

1. Read Knitser's quote and the kascovio launch video-post.
2. Read the public GitHub README (the reset / source-went-private note).
3. Hit live JSON on 15 Sep 2026:
   - `/data/mainnet-live.json`
   - `/data/testnet-10-live.json`
   - `/data/mainnet/templates.json`
   - `/data/testnet-10/templates.json` (44 named templates)
   - `POST /data/testnet-10/artifacts` with garbage JSON
   - `POST /data/mainnet/artifacts` with a fake object (`schema_version` but no `id`)
   - `GET /openapi.json` (artifact routes exist)
4. Walked the on-site decoder samples. They already name `x402 · escrow v2`, Zealous 1.0 vs fork, KForge, KaspaRocket, KCC20, KasWare vaults, from **chain bytes**.
5. Did not paste a real `argentc` `artifact.json` this pass. Did not deploy a covenant. Did not use the trade tab.

## Why I did it that way

The new claim is directional: not "we read the chain" (they already did) but "you hand me your compiler output, I prove the hashes, I tell you which coins already run it."

That is falsifiable with HTTP. If garbage is accepted, the feature is theatre. If missing fields are refused with a compiler-shaped error, the gate is real. Counting 388 fork / 65 of 1.0 without a public list is a separate claim and I treat it as unverified.

## What this actually is

kascov is the open covenant explorer for Kaspa L1. It indexes genesis / transition / burn, names templates from revealed programs, serves a keyless JSON API on mainnet and testnet-10, and (now) accepts an Argent `artifact.json` so names and entrypoints attach to matching on-chain template hashes.

Two SilverScript generations are in play:

- **fork era:** numeric selectors, BLAKE2b template hashes
- **1.0 release:** dispatch tags, BLAKE3 template hashes

The tweet says kascov now reads each generation on its own terms, refuses the whole file if one hash does not reproduce, then names coins whose revealed program hashes to those templates.

The GitHub no longer carries the indexer. README: growth into verifier + trading terminal, codebase taken private, explorer/API promised back, trading machinery stays private. Live site stayed up.

That split is the product: public facts from chain bytes, private matching engine and trade UI.

## What holds

**The refuse-the-file gate is real.**

`POST https://kascov.io/data/testnet-10/artifacts` with `{"name":"nope"}`:

```json
{
  "error": "artifact is not an argentc artifact: missing field `schema_version` at line 1 column 15",
  "ok": false
}
```

HTTP 400. Not 200-with-a-name. Not a silent drop.

A slightly richer fake (`schema_version`, `bytecode`, a dummy templates array) failed next on missing `id`. Same shape: parse the argentc artifact, do not guess.

GET on that path is 405 — POST-only. That is a verifier, not a dump.

**Live index is not a mock.** 15 Sep 2026:

| | mainnet | testnet-10 |
| --- | --- | --- |
| covenants ever | 162,578 | 1,581,111 |
| active | 730 | 94,521 |
| burned | 161,848 | 1,486,590 |
| events | 379,926 | 7,543,537 |
| tip vs processed (mn) | 540351292 / 540351288 | 570970372 (tip) |

Four DAA of lag on mainnet is an honest index, not a stalled one.

**Naming from bytes is already the house style.** Decoder samples include KasWare vault-schnorr, Zealous token (fork) vs Zealous token (1.0), KForge pool/curve, KaspaRocket lp-amm, KCC20 token, x402 escrow v2, genesis0 root. The tweet is not a new philosophy. It is a new **input**: compiler artifact → hash → existing coins.

**Conservative token verdict language on the API** (`verified` only if every event matched a known rule and supply conserved; else `unvalidated` with a reason) is the same discipline KasStacker is trying to copy. kascov had it first on the token directory.

**CORS `*`, no keys, openapi.json.** You can break this from a browser. That is the right surface for a verifier.

**Knitser's sentence is the right product sentence:** two months of reading other people's covenants, then the other direction. Explorer people usually never ship the inverse.

## Opinion

This is the most important Kaspa indexing work on the board. If you can only keep one explorer, keep the one that names a spend by the **entry it called**, not by which output it sat in.

The artifact POST is the correct next step: names should come from the compiler's file, hashes from the file's bytes, coins from the chain. Trust the hash, not the brand.

Then they took the code private. A verifier whose matching rules you cannot read is asking you to trust the operator that the hash function is the one they described. The JSON error is evidence of a parser. It is not evidence of BLAKE2b-vs-BLAKE3 correctness. For a project whose README says "a project built on verification does not get to be vague about itself," the private repo is the vagueness.

Ship the recognizer + hash code as MIT, keep the trading terminal closed if you must. That split is already what the README promises. Do it.

## Result analysis

| Claim | Result |
| --- | --- |
| Paste artifact.json; recompute every template hash from the file's own bytes | **Parser exists and is strict.** Hash-reproduce refuse **not proven** this pass (I never sent a well-formed artifact with a wrong hash). Schema refuse **proven**. |
| Refuse the whole file if one hash does not reproduce | Consistent with the 400s I got (fail closed on first structural error). Whole-file vs per-template is the specified behaviour; I did not reach the hash loop. |
| Then name every coin on chain running those templates | Plausible: they already name from revealed program hashes. Artifact is a label source, not a second index. Unverified without a successful submit that lights up existing coins. |
| Both SilverScript generations on their own terms | Decoder samples already distinguish Zealous fork vs 1.0. Builder page pins `silverc` generation **fork** (`d57e5df`) and `argentc` `05ba4b2` until the worker reports its pair. Fork default on a 1.0-named feature is a footgun — see breakpoints. |
| 388 fork builds and 65 of the 1.0 release, named on TN10 | **Not independently counted.** templates.json has 44 named families, not 453 builds. Builds ≠ templates. No public `GET /artifacts` list (405). Treat 388/65 as the author's census until they publish the list. |
| "A spend is named by the entry it called, not by where it sat in the branch chain" | The right rule for Argent. Cannot confirm from this pass's JSON without a multi-entry artifact. |
| "Every module on the Build tab either works or tells you exactly what is missing" | Matches the 400 error style. Did not click every Build-tab module. |
| "The hash is proven from the bytes. The names are yours." | Correct trust split **if** the hash code is public. Today the names are yours and the hash function is theirs. |
| Open API, verify from chain, no registry | Holds for explorer reads. Artifact submit **is** a submission path — a compiler file, not a token registry. Fine, as long as a failed hash cannot mint a name. |
| Source: explorer coming back, trading stays private | README is honest. GitHub has **one commit**, README only. Live site is the product. Reproducibility is not. |

Mainnet vs TN10 is the maturity tell: mainnet templates are mostly KasWare vaults already burned plus KCC20 and P2SH commitments. TN10 is where named venues and x402 escrow live. Artifact-verify is a **TN10 builder tool** that will matter on mainnet when Argent apps actually ship there.

## Breaking points

1. **Private verifier.** You cannot re-derive "we hashed this with BLAKE3" from chain bytes alone if you do not have the artifact schema and the hash preimage rules. Publish `artifact → template hash` as a tiny MIT crate. Keep the terminal closed.
2. **Schema refuse ≠ hash refuse.** Attackers will send a well-formed artifact whose hashes are copied from a real build and whose `name` is theirs. If names attach because the hashes match **their** file, that is correct (the bytes are the identity). If names attach because the JSON `name` field is pretty, that is a registry. Prove which.
3. **Name squatting.** "The names are yours" is also "the names are whoever submitted first." Need: hash is primary key; name is a label; conflicting labels on the same hash are shown, not overwritten; maybe require a reveal of the same bytecode on-chain before a name sticks.
4. **Fork default vs 1.0 feature.** The Build tab still defaults `silverc` to fork (`d57e5df`, BLAKE2b) "until the worker reports its own pair." A 1.0 artifact submitted against a fork hasher will fail (good) or, worse, collide if they mix rules. Pin the generation from the artifact, never from the page default. The tweet says they now do that. The Build page copy still advertises the fork default.
5. **388 / 65 unlistable.** A verifier that names 453 builds should serve `GET /data/testnet-10/artifacts` or a builds.json. 405 on GET means only the operator can audit the census.
6. **Trading tab vs explorer brand.** `/kascov` is a market UI. The tweet is about compiler artifacts. Mixing "we never guess a token is valid" with a buy button is how people will think kascov approved the market. The token API already refuses to price unmatched programs. Keep that wall on the trade page in 20pt type.
7. **Worker deploys your TN10 covenant.** Build tab: kascov worker funds, signs, broadcasts. That is custodial **testnet** convenience. Document the key, the faucet, the rate limit. A leaked worker key is not a mainnet incident until someone copies the pattern.
8. **Index lag / reorgs.** They expose `tip_daa` vs `processed_daa` and a reorg ledger. Good. Artifact names applied at T and a 2-block rewind need the same fail-closed behaviour as the rest of the index. Not tested here.
9. **Cross-indexer drift.** KasStacker uses kcc20.info; kascov counts 730 live vs ~687 genesis−burn on kcc20. They already have `consistency.json`. If it 404'd or was empty on my first grab, that endpoint needs to stay hot. Disagreement is the product.

## How to improve

- Publish the artifact schema, hash code, and a negative vector: one flipped byte → 400 "template hash did not reproduce."
- `GET` a public catalog of accepted artifacts: hash, declared name, generation (fork/1.0), first-seen DAA, coin count. Then 388/65 becomes checkable.
- Make the hash the identity. Names are aliases. First-submit does not own the hash.
- Kill the Build-tab fork default or put a red banner when the worker and the page disagree.
- Return the explorer/indexer source as promised. Trading binary can stay closed.
- Add an `/simulate` + preflight example that takes a **wrong** artifact hash next to the thief-style KasStacker lesson. The two products should link: KasStacker teaches the script, kascov proves the bytes.

## TN10 this pass

Used. Live feed, templates, artifact POST 400s. No funded deploy, no real `artifact.json`. The 388/65 count remains the author's.

## Send this

This issue. KasStacker is a manual. x402 is a payment RC. kascov is the chain-byte verifier. Do not merge the three into one pep talk.
