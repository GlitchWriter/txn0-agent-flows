# TXN-0 Drop Announcer (Propaganda Machine v1)

The Drop Announcer is the TXN-0 agent responsible for **broadcasting each airdrop** across social channels.  
It pulls confirmed drops from the Ledger of Trust, generates short commentary, and posts to **X (Twitter)** — optionally creating multi-part threads for longer survivor stories.

---

## 🔄 Flow Overview

1. **Schedule Trigger** → runs every ~70 minutes.  
2. **Ledger Query** → pulls rows from the Google Sheets "Ledger of Trust".  
3. **Pick First Pending** → selects the earliest drop marked with `Proof (TXN Link)` but not yet tweeted.  
4. **Compose Tweet** → builds fallback text (drop amount, wallet short form, proof hash).  
5. **GPT Commentary** → generates mythic-style hooks (short, cinematic, loss-themed).  
6. **Normalize JSON** → cleans GPT output into `{commentary, hashtags}`.  
7. **Tweet Guard** → clamps output to ≤280 characters, rotates anchors/tags, ensures proof link fits.  
8. **Safety Gate** → rejects empty or invalid tweets before posting.  
9. **Post Tweet** → publishes via X API.  
10. **Update Ledger** → writes `Tweeted=yes` and stores Tweet ID back to the row.  
11. **Thread Builder** (if Excerpt too long) → splits survivor story into 220-char parts, posts them sequentially, and stores ThreadReplyID.

---

## 🧩 Features

- **Controlled link rotation**: ~20% chance of adding one brand link (DEXTools / Pancake / BscScan / Website).  
- **Mythic fallback pool**: if excerpt is weak or empty, selects from a library of loss-myth one-liners.  
- **Hashtag hygiene**: max 3 tags; always includes `#TXN0` or `$TXNF`.  
- **Threading**: survivor excerpts longer than ~220 chars are broken into `(1/N), (2/N)` style replies.  
- **Proof-first formatting**: every tweet carries a BscScan TX link or wallet short-form.  
- **Human jitter**: built-in random waits to mimic organic posting rhythm.

---

## 🛡️ Guardrails

- Pre-post safety gate ensures valid length (5–280 chars).  
- Duplicate prevention: skips if TweetID or ThreadReplyID already present in ledger.  
- Sequential batching: posts only one pending row at a time.  
- Error handling: rows updated with `Tweeted=error` + reason if post fails.  
- Global state memory tracks thread parents to chain replies properly.

---

## 📊 Example Output

The Glitch Engine TXNOFIED another wallet.
Airdrop → 0xF83d…8127
“Rugs stitched back, one receipt at a time.”
Loss tags: #rugpull #defi
Proof: https://bscscan.com/tx/0xabc123...
#TXN0 #BSC #TXNOFIED

---

## ⚠️ Limitations

- Currently only posts to **X**; Telegram branch exists separately.  
- Requires valid `twitterOAuth2Api` credentials in n8n.  
- Dependent on Google Sheets as the source of truth (Ledger of Trust).  
- Secrets/API keys redacted in JSON; not runnable out-of-the-box.

---

## Dependencies

- **n8n** (automation engine)  
- **Google Sheets API** (ledger access)  
- **OpenAI GPT-4o-mini** (commentary)  
- **Twitter/X API (OAuth2)** (posting)  

---

📌 *This is a technical case study of the TXN-0 automation stack. It is not financial advice or token prom
