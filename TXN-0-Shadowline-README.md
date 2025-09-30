# TXN-0 Shadowline (Mentions & Replies Agent)

**Shadowline** is the TXN-0 agent that keeps the project visible on X by quietly scanning mentions, filtering noise, and choosing whether to reply, like, retweet, or quote.  
Unlike the Drop Announcer (which broadcasts drops), Shadowline focuses on **interaction**.

---

## 🔄 Flow Overview

1. **Identity & State**
   - `Get User-Me` + `Cache User ID` → ensures self-awareness (don’t reply to self).
   - `Load/Rotate State` → daily reset of counters & memories (dedupe, anti-repetition, caps).

2. **Discovery**
   - `Pick Search Query` → builds stable brand-anchored query (`$TXNF`, `@_TXN0`, `txn0.io`, contract).
   - `Search Tweets` → pulls up to 25 fresh posts.
   - `Normalize Search Results` → dedupes, drops self-authored tweets, respects “seen today.”

3. **Mentions Handling**
   - `Build Mentions Query` → `to:_TXN0 -is:retweet`.
   - `Search Tweets-Mentions` → fetches direct mentions.
   - `Filter Mentions` → blacklist filter (politics, religion, spam, mass-tags, <20 followers unless TXN-0 reference).

4. **Scoring & Action Selection**
   - `Score & Pick (≤3)` → assigns numeric score by loss-signal-builder heuristics.
   - `Prefer Suggested Action` → respects scorer hints.
   - `Pick Action (cycle)` → balances LIKE, RT, QUOTE rotation.

5. **Caps & Guardrails**
   - Daily caps (`Cap LIKES/day`, `Cap RT/day`, `Cap POSTS/day`) enforce quotas from the plan.
   - Jitter waits simulate human rhythm.

6. **Reply/Quote Generation**
   - Draft text via `TXN0 Agent-draft qoute` (GPT-4o mini).
   - `Parse Draft Reply` / `Parse Draft qoute` → normalize decision (QUOTE vs SKIP).
   - `Trim Tail Phrases` + `Tail Deduper` → cut repeated endings, remove trailing tags.
   - `IF SKIP (reply)` / `IF SKIP (quote)` → block empty/low-signal outputs.
   - Post via `Reply-Create Tweet` or `Create Tweet-Qoute`.

---

## 🧩 Features

- **Multi-action rotor**: likes, RTs, quotes to avoid spammy repetition.
- **LLM gate**: only quotes/replies when GPT deems context valuable (else SKIP).
- **Blacklist & safety**: avoids politics, religion, violence, scams.
- **Tail deduplication**: no repetitive phrasing across replies.
- **Market awareness**: pulls GeckoTerminal snapshot for planner context.
- **Plan-aware quotas**: OpenAI “Meta Agent” generates a daily plan (quotas, priorities, styles).

---

## 🛡️ Guardrails

- Skips mass-mention spam (>5 @tags).
- Drops low-follower accounts unless they reference TXN-0 directly.
- Caps per day (default): Likes ≤100, RTs ≤20, Quotes ≤6, Replies ≤6 (tunable).
- Never posts bare “QUOTE”/“SKIP” tokens — normalized before send.
- Rotates seen_ids to avoid replying twice to the same tweet.

---

## 📊 Example Behaviors

- Someone mentions TXN-0 with a loss story → LLM may draft an empathetic 1-line reply.  
- A dev tweet about automation → may trigger a quote showing TXN-0’s builder ethos.  
- Random hype “airdrop now” spam → skipped.  
- Market signal post referencing `$TXNF` → may trigger a like or RT.

---

## ⚠️ Limitations

- Requires valid X OAuth2 API credentials.
- Secrets & env vars redacted from JSON — not runnable out-of-box.
- Can under-reply if planner quotas are conservative.
- Strict filters → most actions skip; replies are rare by design.

---

## Dependencies

- **n8n** (automation engine)  
- **Twitter/X API v2** (search, reply, like, RT, quote)  
- **OpenAI GPT-4o mini** (draft gate for replies/quotes)  
- **GeckoTerminal API** (market snapshot for planner context)

---

📌 *This flow is a case study in agentic automation. It’s not a financial product or token promotion.*
