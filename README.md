# Ekstra Pay

> SOL transfer + physical presence proof in one atomic transaction.
> Built on Ekstra's motion architecture by [@Myke_Kript](https://x.com/Myke_Kript)

No gesture = no transaction. Every transfer requires physical human authorization.
The payment and the proof are permanently linked on-chain — atomically.

---

## What Makes This Different From Just Using Phantom

| Phantom alone | Ekstra Pay |
|---|---|
| Wallet click authorizes | Physical gesture authorizes |
| No presence proof | Permanent on-chain proof |
| Transfer only | Transfer + proof atomically |
| A bot could do it | Only a human body can do it |

---

## How It Works

```
Enter recipient + amount
        ↓
Make physical gesture (Flick / Shake / Hold)
        ↓
Gesture is signed with ed25519 keypair
        ↓
Hit "Send + Prove" (30 second window)
        ↓
Phantom opens ONCE
        ↓
Two instructions in one transaction:
  1. SystemProgram.transfer  → SOL moves to recipient
  2. Memo Program            → signed proof written on-chain
        ↓
Both confirmed simultaneously on Solana
        ↓
Receipt appears with Solana Explorer link
```

---

## End-to-End Flow (Devnet)

1. Open Phantom → Settings → Developer Settings → **Switch to Devnet**
2. Open Ekstra Pay → **Connect Wallet**
3. Hit **🪂 Airdrop 1 Devnet SOL** — free test SOL
4. Enter a recipient address and amount
5. Make a gesture — **Flick, Shake, or Hold**
6. Hit **Send + Prove** within 30 seconds
7. Approve once in Phantom
8. Receipt appears → click **View on Solana Explorer**

Both the SOL transfer and the gesture proof appear in the same transaction.

---

## For Mainnet (Real SOL)

1. Switch Phantom to **Mainnet**
2. Toggle **Mainnet** in the Ekstra Pay header
3. Everything else is identical — same flow, real SOL

---

## The Atomic Guarantee

Both instructions are in one Solana transaction. This means:

- If the transfer fails → the proof is NOT written
- If the proof fails → the SOL is NOT sent
- If both succeed → they are permanently linked forever

You cannot have one without the other. The proof is not an afterthought — it is part of the transaction itself.

---

## The Mirage Connection

[Mirage](https://github.com/Michael5512) is a privacy protocol for stablecoin transfers on Ethereum L1 — making transactions indistinguishable from ordinary transfers while remaining fully auditable.

Ekstra Pay demonstrates the **physical authorization layer** that Mirage needs:

- Transaction stays private on-chain
- Sender can prove physical presence via gesture proof
- Result: private transfer that is still humanly attributable

**Privacy + accountability. Simultaneously.**

---

## Run It

```bash
git clone <this-repo>
cd ekstra-pay
python3 -m http.server 8080
# Open http://localhost:8080
```

No npm. No build step. One file.

---

## Features

- **Gesture authorization** — Flick, Shake, or Hold unlocks the send button for 30 seconds
- **Atomic transaction** — transfer + proof in one Solana instruction bundle
- **Devnet + Mainnet** — toggle in the header, works on both
- **Airdrop button** — one-click devnet SOL for testing
- **Balance display** — live wallet balance shown in header
- **Receipt history** — every transaction logged with Explorer link
- **Demo mode** — works without Phantom for UI exploration
- **Phone motion** — DeviceMotionEvent on Android (auto) and iOS (permission prompt)
- **Desktop simulation** — Flick / Shake / Hold buttons for non-mobile testing

---

## Transaction Structure

```
Solana Transaction
├── Instruction 1: SystemProgram.transfer
│   ├── from: sender wallet
│   ├── to:   recipient wallet
│   └── lamports: amount × 1,000,000,000
│
└── Instruction 2: Memo Program
    └── data: {
          protocol: "ekstra-pay",
          gesture:  "FLICK",
          wallet:   "sender pubkey",
          sig:      "ed25519 gesture signature",
          ts:       "ISO timestamp"
        }
```

---

## Architecture

```
Phone / Desktop
    │
    ▼
DeviceMotionEvent → Gesture Classifier (FLICK / SHAKE / HOLD)
    │
    ▼
ed25519 Sign (TweetNaCl) ← Motion Address keypair
    │
    ▼
Send button unlocks (30s window)
    │
    ▼
User submits: recipient + amount + gesture proof
    │
    ▼
Solana Transaction (2 instructions, 1 signature)
    ├── SystemProgram.transfer
    └── Memo (proof)
    │
    ▼
Phantom signs → broadcast → confirmed
    │
    ▼
Receipt + Solana Explorer link
```

---

## Dependencies

- [@solana/web3.js](https://solana-labs.github.io/solana-web3.js) — transaction building (CDN)
- [TweetNaCl](https://tweetnacl.js.org) — ed25519 gesture signing (CDN)
- [Phantom](https://phantom.app) — Solana wallet
- Zero npm packages. Zero build tools.

---

## File Structure

```
ekstra-pay/
├── index.html    ← Everything. One file, zero dependencies.
└── README.md     ← This file.
```

---

## Roadmap

- **USDC transfers** — add SPL token transfer instruction alongside SOL
- **Mirage integration** — route transfers through Mirage privacy layer
- **Batch proofs** — aggregate multiple gesture proofs per session
- **Ekstra runtime motion** — replace DeviceMotionEvent with full Ekstra motion packets for network-verified, trust-scored gesture proofs

---

## Built For

- [Ekstra Build Contest](https://x.com/EkstraAi) — 5 SOL prize
- Submission by [@Myke_Kript](https://x.com/Myke_Kript) — Web3 builder & AI content creator

---

## License

Apache-2.0 — Free to use, fork, and build on.
