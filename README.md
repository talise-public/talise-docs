<div align="center">

# Talise

**Money that moves like a message.**

A gasless US dollar account on Sui that sends by name, settles in under a second, and keeps the amount private. Live on mainnet.

[Web app](https://app.talise.io) · [iOS (TestFlight)](https://testflight.apple.com/join/BFNEPYtM) · [Pitch deck (PDF)](./pitch/Talise-Pitch-Deck.pdf) · [X](https://x.com/taliseio)

</div>

---

## Links for judges

- **Web app:** https://app.talise.io
- **iOS app (TestFlight):** https://testflight.apple.com/join/BFNEPYtM
- **Primary working repository (full build history):** https://github.com/SeventhOdyssey71/talise-main

> These repositories (`talise-frontend`, `talise-mobile`, `talise-contracts`, `talise-infra`, and this `talise-docs`) are the clean, organized submission. The original day to day development happened in the primary working repository above, which we migrated into focused repositories for clarity.

---

## The problem

Moving money across a border still costs an average of 6.4 percent, takes one to five days, and assumes you have a bank. 1.4 billion adults do not. The people who feel it most are the ones who can least afford it.

## The solution

Talise is a dollar account that lives inside a conversation. You sign in with Google, hold dollars, and send them to a name like `vanessa@talise.sui`, not a 0x address. No seed phrase, no gas, no waiting. And you can send privately, with the amount hidden on chain.

## How it works

Four invisible layers, one obvious experience:

1. **Identity.** zkLogin turns a Google account into a self-custodial Sui wallet. The user never sees a key.
2. **Settlement.** Balances are USDsui, a US dollar token on Sui, moved with sub-second finality.
3. **Gas.** Every send is sponsored, so the user pays nothing to transact.
4. **Names.** Everyone claims `name@talise.sui`, a real on-chain SuiNS identity that money routes to.

On top of that core: private (shielded) transfers, claim links, streaming, an on-chain savings vault, and bank cash-out.

## Privacy

Public chains leak every balance and every payment. Talise private send uses a Groth16 shielded pool to hide the amount on chain and break the link between sender and recipient through a relayer. It is real zero-knowledge settlement, live on Sui mainnet today.

## What is live

- Gasless sends to a `name@talise.sui` handle, settling in under a second.
- Private sends with the amount hidden on chain.
- Claim links (money in a link) and streaming (money over time).
- On and off ramps to a bank through licensed partners.
- Public beta at `app.talise.io`, with an iOS build in TestFlight.

## Traction

- 1,900+ names on the waitlist.
- 1,400+ `@handles` already claimed on chain.
- Live on Sui mainnet.

## Repositories

| Repo | What it is | Stack |
|---|---|---|
| [talise-frontend](https://github.com/talise-public/talise-frontend) | Web app and API | Next.js, TypeScript |
| [talise-mobile](https://github.com/talise-public/talise-mobile) | iOS app | Swift, SwiftUI |
| [talise-contracts](https://github.com/talise-public/talise-contracts) | Sui Move packages | Move |
| [talise-docs](https://github.com/talise-public/talise-docs) | This repository: overview and pitch | |

## Architecture at a glance

```
iOS app  ─┐
          ├─►  Talise API (Next.js)  ─►  Sui mainnet (Move packages)
Web app  ─┘          │                        ├─ talise        (send, cheque, stream, vault)
                     │                        ├─ talise-privacy (shielded pool)
                     ├─ zkLogin (identity)    ├─ talise-goals   (savings)
                     ├─ gas sponsor (fees)    └─ talise-yield   (idle balance)
                     └─ licensed ramps (bank in and out)
```

## Pitch

The full pitch is in [`pitch/Talise-Pitch-Deck.pdf`](./pitch/Talise-Pitch-Deck.pdf). The source deck is `pitch/talise-deck.html`.

## License

MIT. See [LICENSE](./LICENSE).
