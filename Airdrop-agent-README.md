# TXN-0 Agent (Airdrop)

This n8n flow powers the AI-driven airdrop engine for TXN-0. It processes story submissions, filters duplicates, validates wallets, and sends tokens based on eligibility and score.

### Key Functions
- Fetches user-submitted loss stories from Google Forms.
- Scores submissions using GPT-4.
- Validates wallet address format and duplication.
- Sends airdrop via token distribution logic.
- Updates internal wallet log and Google Sheet records.
- Notifies users via email and logs actions transparently.

All logic is autonomous and requires no manual approval during normal operation. Internal sheets and logs are not public-facing and are used for agent memory.
