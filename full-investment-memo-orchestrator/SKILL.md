---
name: full-investment-memo
description: Orchestrator for the 3-step investment memo pipeline. Shows which skills to run in which order. Triggers on "investment memo", "full memo", "IC memo", "write the memo", or when Enzo has a data room + multiple calls + references. This skill does NOT do any work itself — it tells you which 3 skills to run in 3 separate sessions.
---

# Full Investment Memo — Pipeline Orchestrator

This is a 3-step pipeline. Each step runs in a **separate Cowork session** for maximum quality.

## The Pipeline

```
Session 1                    Session 2                    Session 3
┌─────────────────┐         ┌──────────────────┐         ┌──────────────────┐
│ evidence-ledger  │   ──▶  │  context-pack    │   ──▶   │ investment-memo  │
│                  │         │                  │         │                  │
│ Extract, tag,    │  Enzo   │ External research│  Enzo   │ Write narrative  │
│ & cross-check    │ reviews │ & validation     │ reviews │ memo for IC      │
│ metrics          │         │                  │         │                  │
└─────────────────┘         └──────────────────┘         └──────────────────┘

Inputs:                      Inputs:                      Inputs:
• Pitch deck                 • Evidence Ledger            • Evidence Ledger
• Call transcripts           • (web research)             • Context Pack
• Data room                                               • Calibration examples
• Reference calls

Output:                      Output:                      Output:
• [Co]_Evidence_Ledger.md    • [Co]_Context_Pack.md       • [Co]_Full_Investment_Memo.md
```

## Why Separate Sessions?

Each step requires a different cognitive mode. Running them in one session degrades quality because:

1. **Context saturation** — by Step 3, the model has 60-80K tokens of prior conversation competing with voice calibration
2. **Cognitive mode conflict** — extraction rules ("no synthesis") conflict with memo rules ("synthesize everything")
3. **Traceability enforcement** — separate sessions force Step 3 to actually READ the files, not rely on conversation memory

## How to Run

### Step 1: Evidence Ledger

**Trigger:** "Generate evidence ledger for \[Company]"
**What it does:** Extracts and classifies every fact from all inputs. Cross-checks metrics against raw data room files. No external research.
**Review:** Check for completeness, correct tagging, metric cross-checks, contradictions caught.

### Step 2: Context Pack

**Trigger:** "Generate context pack for \[Company]"
**What it does:** Orchestrates 3 atomic skills (understand-industry → competitive-analysis → market-research) in one session, then assembles findings and reconciles against the Ledger. 30-50+ searches total.
**Review:** Check verification results, competitive intel quality, market data, team verification.

### Step 3: Investment Memo

**Trigger:** "Write the investment memo for \[Company]"
**What it does:** Synthesizes Ledger + Context Pack into narrative memo. Applies voice calibration. Scores 8 Moats. Builds TAM.
**Review:** Check voice, thesis clarity, evidence discipline, risk specificity.

### Step 4 (optional): .docx

**Trigger:** "Convert the memo to docx"
**What it does:** Formats as Word document with Reach Capital styling.

## Comparison: Old Monolith vs. New Pipeline

| Dimension                   | Old (1 skill, 1 session)               | New (3 skills, 3 sessions)     |
| --------------------------- | -------------------------------------- | ------------------------------ |
| Context for memo writing    | 60-80K tokens of noise                 | \~15-20K tokens, all signal    |
| Voice calibration attention | Competing with extraction instructions | Fresh, focused, full attention |
| Human review gates          | Afterthought                           | Built into the architecture    |
| Traceability                | Theoretical (conversation memory)      | Enforced (file reads)          |
| Debugging                   | Which of 853 lines is wrong?           | Which skill is wrong?          |
| Iteration                   | Re-run everything                      | Re-run just the broken step    |

## Related Skills

| Skill                       | When to use instead                             |
| --------------------------- | ----------------------------------------------- |
| **first-look**              | Initial screening. Deck or notes only. 1 page.  |
| **pre-ic-memo**             | After 1 call + deck. 4 pages. Includes 8 Moats. |
| **founder-turndown-emails** | If memo leads to a pass.                        |