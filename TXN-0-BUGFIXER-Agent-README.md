# TXN-0 Bug Fixer (Recovery Flow)

This is a safety utility flow used to recover, reset, or reprocess submissions in the TXN-0 ecosystem.

### Key Functions
- Detects and resets stuck rows marked as "In Progress."
- Reverts row status for reprocessing when needed.
- Safely updates Google Sheets and internal records.

This flow ensures the airdrop system can recover gracefully from interruptions without compromising data integrity or agent behavior.
