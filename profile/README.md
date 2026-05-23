
# GrantOS v3

> The first onchain grant protocol that enforces milestone delivery with cryptographic proofs — not trust, not screenshots, not committee opinion. The contract verifies. The code runs.

![Arbitrum](https://img.shields.io/badge/Arbitrum-One-blue?style=flat-square)
![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/status-live-brightgreen?style=flat-square)
[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen?style=flat-square&logo=vercel)](https://grant-os-frontend-ruby.vercel.app)

🚀 **Live Demo:** [grant-os-frontend-ruby.vercel.app](https://grant-os-frontend-ruby.vercel.app)


---

## The Problem

Every major DAO — Arbitrum, Optimism, Uniswap, Compound — distributes millions in grants every year. The process is identical across all of them:

1. Builder applies in a governance forum
2. Committee votes to award the grant
3. Funds sent directly to the builder wallet
4. DAO posts a spreadsheet and hopes for the best

**Step 4 is the problem.** Once funds leave the treasury, enforcement becomes social. A builder who misses milestones faces a forum post, maybe a tweet — but they keep the money.

Even DAOs that attempt milestone-based grants face a deeper failure: evidence verification is entirely subjective and gameable. A builder submits a GitHub link. The committee opens it. They see a PR. They approve it. But the PR could be opened against a private repo that gets deleted. The author could be a different account. The work could be copy-pasted from another developer.

**GrantOS v3 eliminates the subjective evidence layer entirely.**

---

## The Solution

GrantOS v3 is a cryptographic grant enforcement layer. Milestones are defined, funds are locked in escrow, and releases are automatic. But v3 goes further — it does not trust a builder's word. It verifies a cryptographic proof.

```
Builder clicks Generate Proof
        ↓
vlayer Web Prover calls GitHub API via TLSNotary MPC-TLS
        ↓
Cryptographic proof generated — data confirmed from GitHub's servers
        ↓
ZK proof submitted to GrantEscrow.sol
        ↓
WebProofVerifier contract verifies proof onchain
        ↓
Milestone advances to Submitted state
        ↓
Committee sees ZK Verified ✓ — not a link, not a screenshot
        ↓
Quorum approves — payment releases automatically
```

---

## Core Features

### ⚡ ZK Proof Milestone Submission
Builders generate a cryptographic web proof via Noir ZK Coprocessor confirming their GitHub PR exists, is merged, and is authored by their wallet-linked account. The smart contract verifies the proof. No human reads anything.

### 🔐 ZK Identity Binding
One-time permanent proof linking a wallet address to a verified GitHub account with real contribution history. Sybil attacks on grant programs are dead.

### 🛡️ Proof-Gated Slashing
Committees cannot slash without first submitting an onchain warning EAS attestation. Builders have cryptographic due-process rights enforceable at the contract level. No warning onchain means no slash is possible.

### 💧 Streaming Milestone Payments
Approved milestones stream USDC per second via Superfluid. The stream is cancellable and slashable at any point. Builders earn exactly what they earned — to the millisecond.

### 🤖 AI Milestone Verifier
GPT-4o reads the evidence URL and writes a plain-English analysis into the EAS attestation. Advisory only. The ZK proof is the enforcer. The AI is the assistant.

### 🏆 Builder Reputation Score
A permanent, public, tamper-proof score derived from the builder's full onchain attestation history. No admin controls it. No committee member can modify it. It is what the blockchain says they did.

---

## Protocol Stack

| Layer | Technology |
|---|---|
| Blockchain | Arbitrum One |
| ZK Web Proofs | Noir ZK Coprocessor|
| Attestations | Ethereum Attestation Service (EAS) |
| Streaming Payments | Superfluid Protocol |
| ZK Computation | SP1 zkVM (Succinct) |
| Frontend | Next.js 15 App Router |
| Wallet | RainbowKit + Wagmi + Viem |
| State | Zustand |
| Styling | Tailwind CSS |
| AI Verifier | GPT-4o |

---

## Smart Contracts

| Contract | Description |
|---|---|
| `GrantEscrow.sol` | Core escrow logic — milestone management, voting, payment release, slashing |
| `GrantIdentityRegistry.sol` | ZK identity bindings — wallet to GitHub handle |

All contracts deployed on Arbitrum One. All verified on Arbiscan. No upgrade proxies. No hidden admin functions. What you see is what runs.

---

## Application Routes

| Route | Role | Description |
|---|---|---|
| `/` | Public | Onboarding — role detection and routing |
| `/verify` | Builder | ZK identity binding — one time |
| `/dashboard` | Builder | Active grants and milestone actions |
| `/my-grants` | Builder | Complete grant history |
| `/profile` | Builder | Private identity and earnings hub |
| `/grants/new` | Committee | Create a new grant |
| `/committee` | Committee | Milestone review and voting |
| `/tasks` | Committee | Personal urgent action queue |
| `/dao` | DAO Admin | Protocol overview and alerts |
| `/treasury` | DAO Admin | Complete financial command center |
| `/grants/[id]` | Public | Full grant detail — no wallet required |
| `/builders/[address]` | Public | Builder public profile — no wallet required |
| `/guidelines` | Public | Protocol documentation |

---

## User Roles

| Role | What they do |
|---|---|
| **Builder** | Completes ZK identity binding, generates ZK proofs for milestone submissions, receives streaming or lump-sum USDC payments, builds onchain reputation |
| **Committee Member** | Reviews ZK proof status and AI verdicts, votes approve or reject with wallet transactions, issues onchain warnings before slashing, manages task queue |
| **DAO Admin** | Monitors protocol health, treasury metrics, and grant program performance across all active grants |
| **Public Observer** | Views any grant's proof history, any builder's reputation score and ZK Verified status — no wallet required |

---

## How Proof-Gated Slashing Works

```
Milestone misses deadline
        ↓
Committee identifies overdue milestone
        ↓
Committee issues warning EAS attestation onchain
        ↓
Builder sees warning immediately — live 24hr countdown
        ↓
24 hours pass
        ↓
Slash button activates
        ↓
GrantEscrow.sol validates: warning attestation older than 24hrs?
        ↓
If NO  → transaction reverts with WarningRequired()
If YES → slash executes, USDC returns to treasury
        ↓
Builder reputation updates: -5 warning, -15 slash
```

No warning onchain. No slash possible. This is what trustless governance looks like.

---

## Builder Reputation Scoring

| Event | Points |
|---|---|
| Milestone approved, on time | +10 |
| Milestone approved, late | +4 |
| ZK proof submitted | +2 |
| Milestone rejected | -3 |
| Warning received | -5 |
| Milestone slashed | -15 |

Score clamped 0–100. Letter grades: A (90–100), B (75–89), C (60–74), D (40–59), F (below 40). Derived entirely from EAS attestation history. Zero admin control. Zero manual override. Ever.

---

## Getting Started

### Prerequisites
- Node.js 18+
- A WalletConnect Project ID
- A GitHub OAuth App (Client ID + Secret)
- An OpenAI API key (GPT-4o for AI verifier)

### Installation

```bash
git clone https://github.com/your-org/grantos-v3
cd grantos-v3
npm install
```

### Environment Variables

```bash
cp .env.example .env.local
```

```env
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
OPENAI_API_KEY=
NEXT_PUBLIC_IDENTITY_REGISTRY_ADDRESS=
NEXT_PUBLIC_GRANT_ESCROW_ADDRESS=
NEXT_PUBLIC_REPUTATION_REGISTRY_ADDRESS=
NEXT_PUBLIC_WEB_PROOF_VERIFIER_ADDRESS=
TREASURY_ALERT_THRESHOLD=
```

### Run Locally

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000)

---

## How It Differs From Every Other Grant Protocol

| Feature | Traditional DAO Grants | GrantOS v3 |
|---|---|---|
| Evidence verification | Committee reads a link | Smart contract reads a ZK proof |
| Identity verification | Self-reported | Cryptographically bound onchain |
| Slashing due process | Social pressure | Contract-enforced 24hr warning |
| Payments | Lump sum on trust | Streaming per second via Superfluid |
| Reputation | Forum history | Tamper-proof onchain attestation score |
| Sybil resistance | None | ZK identity binding — one wallet, one GitHub |
| AI assistance | None | GPT-4o analysis stored in EAS attestation |

---

## The Demo in One Sentence

> *"This builder did not submit a GitHub link. They submitted a cryptographic proof. The smart contract just read GitHub — not a human, not a committee member, not an AI. A ZK proof, verified onchain, in the same transaction. That is new. That is GrantOS v3."*

---

*Built on Arbitrum One. Powered by Noir ZK Coprocessor, EAS, and Superfluid.*
