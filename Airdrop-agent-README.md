# TXN-0 Airdrop Agent (n8n flow)

This flow powers the AI-assisted airdrop engine for TXN-0. It ingests loss-story submissions, validates wallets, filters duplicates, and sends **symbolic on-chain acknowledgements**, logging every action for auditability.

> Current status (Sep 2025): **200+ drops executed** and logged; **~330** queued. Numbers are approximate and updated periodically.

---

## Key Functions
- **Intake** → Google Form ➜ Google Sheets review queue  
- **Scoring** → LLM (GPT/Claude) + rules to flag spam/bots/low-signal entries  
- **Wallet handling** → checksum + format validation  
- **De-duplication** → compare against `wallet-log.json` (authoritative ledger)  
- **Send** → token transfer to validated wallets (sequential, single-row gate)  
- **Log** → update `wallet-log.json` and row status in the sheet  
- **Notify** → optional email to participant  
- **Comms** → optional Telegram/X posting with TX hash + truncated wallet

---

## Safety & Approvals (human-in-the-loop)
The system is designed to run end-to-end automatically, **but** during live operation a **Telegram approval gate** is kept as a safety brake.  
- Purpose: catch edge cases (weird stories, risky wallets, duplicates) before sending.  
- Mode: can be flipped to fully autonomous; kept manual-gate for now to avoid accidental drains.  
- Processing is **sequential** (one row at a time) to guarantee idempotency.

> Previous wording said “requires no manual approval.” That’s not accurate. It’s semi-autonomous with an optional human gate by design.

---

## Outputs in the Wild
For transparency, actions can be mirrored to public channels:
- **X (Twitter)**: short commentary with **TX hash link** and truncated wallet (e.g., `0xF83d…8127`)  
- **Telegram**: drop confirmations / announcements  
- **On-chain**: verifiable transaction on BscScan

This creates a public trail that matches: **tweet ↔ TX on BscScan ↔ ledger entry**.

---

## Idempotency & Guardrails
- Single-row gate prevents double processing.  
- All comparisons use **checksummed** addresses.  
- Duplicate wallets are labeled and skipped.  
- Most social branches return `SKIP` unless strict conditions are met (avoid spam).

---

## Limitations / TODO
- Locking could be stronger than single-row gate for high throughput.  
- Secrets/ENV are redacted, so this is **not** a 1-click runnable package.  
- Gas/scale economics not optimized for very high volume yet.  
- Flow screenshots are provided instead of raw JSON to avoid exposing keys/APIS.

---

## Screenshots / Artifacts
See the `images/` folder for the airdrop flow diagram and node snapshots with captions.

*This document is a technical case study, not financial advice or a token promotion.*
