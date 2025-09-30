# TXN-0 Airdrop Agent (n8n flow)

This flow powers the AI-assisted airdrop engine for TXN-0. It ingests loss-story submissions, validates wallets, filters duplicates, and sends symbolic on-chain acknowledgements, logging every action for auditability.

---

## 📈 Current Status (Sep 2025)
- 200+ drops executed and logged  
- ~330 queued (numbers approximate)  
- Flow runs sequentially on a Contabo VPS  

---

## 🔄 Key Functions
- **Intake** → Google Form ➜ Google Sheets review queue  
- **Scoring** → LLM (GPT/Claude) + rules to flag spam/bots/low-signal entries  
- **Wallet Handling** → checksum + format validation  
- **De-duplication** → compare against `wallet-log.json` (authoritative ledger)  
- **Send** → token transfer to validated wallets via `send-drop.js` (sequential, single-row gate)  
- **Log** → update `wallet-log.json` and row status in the sheet  
- **Notify** → optional email to participant  
- **Comms** → optional Telegram/X posting with TX hash + truncated wallet  

---

## 🛡️ Safety & Approvals
The system is designed to run end-to-end automatically, but during live operation a **Telegram approval gate** is kept as a safety brake.  

- **Purpose** → catch edge cases (weird stories, risky wallets, duplicates) before sending  
- **Mode** → can be flipped to fully autonomous; kept manual-gate to avoid accidental drains  
- **Sequential Processing** → one row at a time to guarantee idempotency  

Previous drafts said “requires no manual approval.” That’s not accurate.  
It’s **semi-autonomous with an optional human gate** by design.

---

## 📊 Outputs in the Wild
For transparency, actions can be mirrored to public channels:

- **X (Twitter)** → short commentary with TX hash + truncated wallet (e.g., 0xF83d…8127)  
- **Telegram** → drop confirmations / announcements  
- **On-chain** → verifiable transaction on BscScan  

This creates a public trail that matches:  
**tweet ↔ TX on BscScan ↔ ledger entry**.

---

## 🧩 Idempotency & Guardrails
- Single-row gate prevents double processing  
- All comparisons use **checksummed addresses**  
- Duplicate wallets are labeled and skipped  
- Most social branches return `SKIP` unless strict conditions are met (to avoid spam)  

---

## ⚠️ Limitations / TODO
- Locking could be stronger than single-row gate for high throughput  
- Secrets/ENV redacted → not a 1-click runnable package  
- Gas/scale economics not optimized for very high volume yet  
- Flow JSON not included to avoid exposing keys/APIs  

---

## 📷 Screenshots / Artifacts
See `TXN-0 Airdrop Agent.png` for the main flow diagram, plus node snapshots with captions.  

---

**Note:** This document is a technical case study, not financial advice or a token promotion.
