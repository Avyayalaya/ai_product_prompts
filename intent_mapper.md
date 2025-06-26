# 🎯  Intent → Action Worksheet  (v3 – supports multiple hypotheses)

> **Mission:** turn ANY exec quote—simple or sprawling—into one or more build-ready specs, each with its own signal plan, UX hook, metric, and 14-day task list.

---

## 0. Primer (60-sec recap)

| Layer | What We Change At That Layer |
|-------|------------------------------|
| **Signals / Metadata** | Raw flags like `@mentioned_exec`, `deadline_hours`, `owner_tier`. |
| **Prompt / Logic** | The single line the LLM or rule engine reads (e.g., “Prioritise OWNER_TIER: MINE”). |
| **Retrieval / Ranking** | Boost, filter, or tier rules before results hit the screen. |
| **UX & Copy** | Where/ how the user sees it—shelf, badge, tooltip. |

**Golden Metrics** we care about (pick ONE per hypothesis):

- Precision @ Top 5   - Time-to-Surface-Insight (p95)  
- Decision-shelf click-rate   - Noise Rate (< 10 % is a hard ceiling)

> **Ship or kill rule:** a change survives only if it *improves its chosen metric* for 7 live days **and** keeps Noise < 10 %.

---

## 1. Raw Intent  
> “{{ PASTE THE FULL EXEC QUOTE }}”

---

## 2. Break Intent into Mini-Themes  
*(Use multiple rows if the quote has several asks.)*

| # | Mini-Theme (one line) | Primary Metric to Move |
|---|-----------------------|------------------------|
| 1 | | |
| 2 | | |
| 3 | | |

---

## 3. Build a Hypothesis for **each** Mini-Theme  

> **Template:**  
> “If __________ (trigger / rule) then __________ (metric ↑/↓).  
> We will measure success with **_______ (metric)**.”

### 1️⃣ Hypothesis  
…

### 2️⃣ Hypothesis  
…

*(Add 3, 4, … as needed—keep each laser-focused and measurable.)*

---

## 4. Mapping Table — repeat **once per hypothesis**

### Hypothesis # __  

| Layer | What We Will Add or Change (short bullets) |
|-------|--------------------------------------------|
| **Signals / Metadata** | • … • … |
| **Prompt / Logic** | • … |
| **Retrieval / Ranking** | • Boost? tier? filter? |
| **UX & Copy** | • Shelf? badge? tooltip copy? |
| **Edge-Case Safeguard** | • Noise clamp, mute path |

---

## 5. Success Definition — per hypothesis

| Metric | Baseline | Target | Window |
|--------|----------|--------|--------|
|        |          |        | 7 days |

*(Noise must stay < 10 % or flag auto-rolls back.)*

---

## 6. Mini-Task List (≤ 14 calendar days) — per hypothesis

1. Data / tagging — …  
2. Backend / logic — …  
3. Front-end / UX — …  
4. Flag & A/B setup — …  
5. Dashboard tile — …

> *If any task > 14 days, split it or defer; the whole point is sprint-size bets.*

---

### 📍 FULLY-WORKED EXAMPLE — two hypotheses from one quote

> **Raw quote**  
> “I need to see anything waiting for my sign-off **first**, and I keep missing mandatory trainings—those should poke me until I finish them.”

| # | Mini-Theme | Metric |
|---|------------|--------|
| 1 | Decision urgency (sign-offs) | Decision-shelf click-rate |
| 2 | Training nudges | Training completion rate |

---

#### 1️⃣  Hypothesis (sign-offs)  
“If artefacts that contain an approval verb *and* @-mention me (or expire in < 48 h) are tagged **CRITICAL** and pinned first, then Decision-shelf click-rate will rise by ≥ 10 pp.”

##### Mapping  
*Signals* `has_approval_keyword`, `@mentioned_exec`, `deadline_hours`  
*Prompt* “Rank ALERT_LEVEL: CRITICAL highest.”  
*Ranking* Tier sort CRITICAL first.  
*UX* Red 🚨 badge, Critical shelf slot #1.  
*Safeguard* Max three CRITICAL cards; overflow collapses.

##### Success  
Click-rate 42 % → **≥ 52 %** in 7 days; Noise < 10 %.

##### 14-day Tasks  
1. Regex + @mention extractor 2 d  
2. Rule & tag engine 2 d  
3. Badge in FE 3 d  
4. Flag & A/B 1 d  
5. Dashboard tile 1 d

---

#### 2️⃣  Hypothesis (training)  
“If overdue trainings reappear every Monday in the Updates shelf, then training-completion rises 15 pp within 30 days.”

##### Mapping  
*Signals* `training_due_flag`, `due_date`  
*Prompt* “Re-surface TRAINING_DUE every Monday.”  
*Ranking* Weekly cron adds ALERT_LEVEL: TRAINING_DUE.  
*UX* Yellow 📚 badge, Updates shelf top.  
*Safeguard* Auto-removes once `completed_flag` true.

##### Success  
Completion 60 % → **≥ 75 %** by day 30; Noise unchanged.

##### Tasks  
1. Training flag ingest 2 d  
2. Weekly cron + tag 1 d  
3. FE badge 2 d  
4. Flag & A/B 1 d  
5. Completion dashboard 1 d

---
