# Talise shielded pool — privacy and trust model

This document states plainly what the Talise shielded pool hides, what it does
not hide, and what you have to trust for the privacy guarantee to hold. It is
written so a technical reviewer can judge the claim without reading the
circuit. We would rather understate the guarantee than oversell it.

The shielded pool is `talise-privacy`, a Groth16 zero-knowledge pool deployed on
Sui mainnet (modules `shielded_pool`, `merkle`, `note_account`, `proof`,
`ext_data`). It uses a height-26 Merkle commitment tree, a nullifier set to
prevent double-spends, and encrypted notes.

## What a shielded transfer hides

- **The amount.** A transfer inside the pool moves value between commitments;
  the amount is committed and proven in zero knowledge, not written in clear on
  chain.
- **The sender-to-recipient link.** Spending a note reveals a nullifier, not
  which commitment it came from. Within the pool, an on-chain observer cannot
  tie a specific deposit to a specific withdrawal from the transaction graph
  alone.

## What it does NOT hide (be explicit)

1. **The pool boundaries are public.** Depositing into the pool and withdrawing
   out of it are ordinary on-chain actions. The value entering and the value
   leaving are visible at the edge. Privacy is *inside* the pool, between the
   deposit and the withdrawal — not at the moment you fund or exit it.
2. **The anonymity set is the pool, and it is early.** Your unlinkability is
   only as strong as the number of other notes you blend with. The pool is in
   beta and currently holds on the order of dozens of notes. A small anonymity
   set means a determined analyst, correlating amounts and timing, can narrow
   the candidate set. This honestly improves only with usage; we do not claim
   strong anonymity at today's volume.
3. **Amount and timing correlation at the edges.** If you deposit an unusual
   amount and withdraw the same unusual amount shortly after, those two public
   edge events can be linked by inspection even though the in-pool hop is
   private. Equal-in/equal-out and tight timing leak.
4. **Network-level metadata.** Talise sends are gasless: a sponsor co-signs and
   submits the transaction. That sponsor (and any network observer) sees the
   submitting connection and timing. This does not break the on-chain
   cryptographic unlinkability, but it is a metadata observer outside the
   on-chain model, and we name it rather than ignore it.
5. **Recovery / key handling.** To make a consumer wallet recoverable through
   zkLogin sign-in, viewing-key material can be escrowed server-side
   (`shield_key_escrow`). This is a deliberate usability-vs-privacy trade: it
   means the operator could, in principle, derive a user's view of their own
   notes. It is disclosed here so the trust boundary is explicit; hardening this
   (client-held keys, optional escrow) is on the roadmap.

## What you must trust

- **The trusted setup.** Groth16 requires a one-time trusted setup per circuit.
  If the setup's secret ("toxic waste") were retained, an attacker could forge
  proofs — which would break *soundness* (minting value that was not
  deposited), not *privacy* (it would not deshield existing transfers). The
  current setup is a project-run setup appropriate for a beta, not a large
  public multi-party ceremony. A public, audited ceremony is required before
  this should be relied on for material value, and is on the roadmap.
- **The circuit and contracts are unaudited.** The Move packages and the
  proving circuit have not had a third-party security audit. Treat the pool as
  experimental.
- **The Sui base layer.** Standard assumption: Sui consensus and the Move
  runtime behave correctly.

## Roadmap to harden the guarantee

These are the concrete steps that turn the current beta into a credible privacy
system, in priority order:

1. **Grow the anonymity set** — the single biggest lever; privacy is a function
   of pool usage.
2. **Fixed denominations** for deposits and withdrawals, so amounts stop being
   a correlation signal at the edges.
3. **Batched / delayed withdrawals and decoys** to break timing correlation.
4. **Independent, multiple relayers** (and a self-relay path) so no single
   submitter sees all traffic.
5. **Client-held viewing keys** with escrow strictly opt-in.
6. **A public, audited trusted-setup ceremony** and a third-party audit of the
   circuit and Move contracts.

## Summary

Today the pool gives a real, working in-pool amount-and-link guarantee on
mainnet, with a small anonymity set and explicit operator/setup trust
assumptions. It is honest beta cryptography, not a finished privacy product. The
items above are what close the gap, and we would rather a reviewer hold us to
this list than to a marketing claim.
