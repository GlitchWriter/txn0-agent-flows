# TXN-0 BUGFIXER Agent (Error Recovery & Watchdog)

The BUGFIXER agent is a monitoring workflow that keeps the TXN-0 airdrop pipeline healthy.  
Its job is to detect submissions stuck in `In Progress`, alert admins, and safely reset them for reprocessing.

---

## 🔄 Flow Overview

1. **Schedule Trigger** → runs every 10 minutes to scan the Google Sheets review queue.  
2. **Filter** → selects rows marked `In Progress`.  
3. **Detect Stuck Rows** → checks if a row has been locked longer than ~20 minutes.  
4. **Alert Telegram** → sends a ⚠️ warning with inline reset button.  
5. **Reset** → if approved, resets the row to `Pending` and logs the action.  
6. **Confirmation** → sends a Telegram message confirming the reset.  
7. **Audit** → marks `Alert_Sent=Yes` in the sheet to prevent repeat alerts.

---

## 🛡️ Safety Features

- **Human-in-loop reset** → admin must approve reset via Telegram bot.  
- **Age check** → avoids resetting rows that are only briefly in-progress.  
- **Anti-loop** → once an alert is sent, the row won’t trigger duplicate alerts.  
- **Confirmation** → all resets confirmed back in Telegram for accountability.

---

## 📊 Example Behavior

- Row 231 marked `In Progress` for >20 min.  
- BUGFIXER sends Telegram:  
- Admin clicks reset → row flipped back to `Pending`.  
- Telegram sends: “Row 231 has been reset to Pending.”

---

## ⚠️ Limitations

- Time threshold is static (20 min).  
- Requires active Telegram bot connection.  
- Only resets via row number; does not validate deeper logic.  

---

## Dependencies

- **n8n** → orchestration  
- **Google Sheets API** → read/write queue rows  
- **Telegram Bot API** → alert + inline reset approval
