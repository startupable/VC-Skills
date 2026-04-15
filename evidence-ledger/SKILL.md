---
name: evidence-ledger
description: Extract and classify every fact from deal inputs (pitch deck, call notes, data room, references) into a tagged, structured Evidence Ledger. This is Step 1 of 3 in the investment memo pipeline. Triggers on "evidence ledger", "extract the evidence", "start the ledger", "process the inputs", or when Enzo has deal materials ready and says "let's start the memo process." Do NOT use for research or verification (use context-pack). Do NOT use for writing the memo (use investment-memo).
---

# Evidence Ledger Generator

Extract, classify, and cross-check every meaningful fact from all deal inputs. No external research, no synthesis, no investment judgment.

Think like a **diligent junior analyst** processing a data room for a partner. You read everything, pull the facts that matter, flag where numbers don't add up, and organize it so the next person can work fast. You don't extract 12 rows from a revenue graph — you extract the insight (growth rate, trajectory, inflection points) and cross-check it against the raw data if it's in the room.

## When to Use

- Enzo has deal materials ready (deck, call notes, data room, references)
- Starting the investment memo pipeline (Step 1 of 3)
- NOT for verifying claims (that's the context-pack skill, Step 2)
- NOT for writing the memo (that's the investment-memo skill, Step 3)

## Pipeline Position

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ EVIDENCE LEDGER │ ──▶ │  CONTEXT PACK    │ ──▶ │ INVESTMENT MEMO  │
│ ★ YOU ARE HERE  │     │  (Step 2)        │     │  (Step 3)        │
│                 │     │  Separate session │     │  Separate session │
└─────────────────┘     └──────────────────┘     └──────────────────┘
```

**Output from this skill** feeds directly into the context-pack skill in a **separate Cowork session**. The Ledger file is the interface.

## Input Requirements

**Gather before starting:**

- Pitch deck (.pptx or .pdf)
- Call notes, transcripts, or Granola meeting summaries
- Data room materials (financials, traction, cohort data, roadmap)
- Customer/operator/expert reference call notes
- Cap table or funding history
- Enzo's initial take or IC questions

**If inputs are incomplete:** Proceed with what's available. Flag gaps. Never fabricate.

**Always search Granola** by company name and founder names to retrieve meeting transcripts. These are often the richest source of founder claims, reference signals, and deal context. Include all relevant transcripts as inputs before starting extraction.

---

## How to Tag Facts

Every fact gets THREE things: a sequential ID, a source tag, and a confidence tag.

**Format:** `[E#] [Source] [Confidence] Fact`

### Source Tags

| Tag | Meaning | Example |
|-----|---------|---------|
| `[Deck:S#]` | Pitch deck, slide number | `[Deck:S12]` |
| `[DR:filename]` | Data room file | `[DR:cohort-retention.xlsx]` |
| `[Call:Name:Date]` | Founder call | `[Call:Antonio:2025-06-15]` |
| `[Ref:Name]` | Reference call | `[Ref:Andy McLoughlin]` |
| `[Granola:Title]` | Granola meeting transcript | `[Granola:Bem Intro Call]` |

### Confidence Tags

These classify the *type* of claim — NOT whether it's true. Verification happens in the Context Pack (Step 2).

| Tag | What it means | Examples |
|-----|--------------|---------|
| `[HARD]` | Observable company fact — directly verifiable from product, data room, or public records | Headcount, customer count, founding date, product features live today, funding raised |
| `[METRIC]` | Calculated business metric — depends on methodology, time window, denominator choices | NRR, churn, growth rate, CAC, LTV, gross margin, burn multiple |
| `[MKT]` | Market claim — founder's assertion about the market, industry, or TAM | "Market is $X billion", "Growing at Y% CAGR", buyer behavior claims |
| `[COMP]` | Competitive claim — founder's assertion about what competitors do, earn, or lack | "Competitor X has Y% market share", "Their product can't do Z" |
| `[REG]` | Regulatory/policy claim — assertion about compliance, legal frameworks, policy changes | "New state mandate requires X", "HIPAA requires Y" |
| `[REF-SIGNAL]` | Reference signal — what a reference said about the founder, product, or market | Investor opinions, customer testimonials, operator assessments |

**Examples:**

```
[E14] [Call:Christopher:2025-12-09] [METRIC] "Net dollar retention is 140% across our first 6 cohorts"
[E27] [Deck:S8] [MKT] "US home services market is $600B and growing 8% annually"
[E31] [Call:Christopher:2025-12-09] [COMP] "ServiceTitan can't handle pest control workflows natively"
[E45] [DR:cohort-data.xlsx] [HARD] Monthly revenue by cohort: Jan $12K, Feb $18K, Mar $31K...
```

---

## Ledger Structure

```markdown
# Evidence Ledger: [Company Name]
Generated: [Date]
Inputs processed: [list all files, calls, references processed]
Total evidence items: [count]

---

## Company & Product
- [E1] [Source] [HARD] Fact
- [E2] [Source] [HARD] Fact

## Team
- [E3] [Source] [HARD] Fact
- [E4] [Ref:Name] [REF-SIGNAL] What reference said

## Market & Competitive
- [E5] [Source] [MKT] Market claim
- [E6] [Source] [COMP] Competitive claim
- [E7] [Source] [REG] Regulatory claim

## Traction & Financials
- [E8] [Source] [HARD] Observable metric (customer count, ARR as stated)
- [E9] [Source] [METRIC] Calculated metric — needs methodology check
- [E10] [DR:filename] [HARD] Data room number

## Terms & Cap Table
- [E11] [Source] [HARD] Fact

## Reference Signals
- [E12] [Ref:Name] [REF-SIGNAL] What they said about founder/product/market

## Metric Cross-Checks
- [M1] CONFIRMED: [metric] [E#] — raw data [E#] confirms (within 5%). Methodology: [how calculated]
- [M2] DIVERGENT: [metric] [E#] claims X — raw data [E#] shows Y. Reason: [methodology difference]
- [M3] UNVERIFIABLE: [metric] [E#] — no raw data in inputs. Needs external verification.

## Contradictions & Flags
- [F1] Contradiction: [E# vs E#] — [description of conflict]
- [F2] Gap: [description of missing information]
```

---

## Ledger Rules

1. **Every fact gets a source tag AND a confidence tag.** No unattributed, unclassified claims.
2. **Contradictions get their own section** with cross-references to conflicting E# entries.
3. **Direct quotes stay in quotes.** Paraphrases don't get quotes.
4. **Roadmap items labeled as such.** Never mix roadmap with shipped capabilities. Use: `[E#] [Source] [HARD] ROADMAP: [feature]`
5. **Process all inputs systematically.** Go through each document/transcript fully. Don't skip or summarize.
6. **When a [METRIC] claim has raw data in the data room, cross-check it.** Extract the claim, extract the raw data, recalculate, and report the result in the Contradictions & Flags section. Show your math. Example: `[F#] METRIC CHECK: Founder claims 140% NRR [E14] — cohort data [E45] shows 118% NRR (calculated as: [methodology]). Divergence: founder used trailing 3 months, full data shows trailing 12.`
7. **When a [METRIC] claim has NO raw data**, flag it: `[F#] [METRIC] without raw data: E# — unverifiable from inputs, needs external verification in Context Pack`.
8. **For charts and graphs**, extract the insight — not every data point. Pull the trend, growth rate, start/end values, and any inflection points. If the raw data exists in the data room, cross-check the headline number against it (rule 6). Example: `[E#] [Deck:S8] [METRIC] Revenue chart shows ~8x growth Jan-Dec, ending ~$80K MRR. Cross-check against [DR:financials.xlsx].`
9. **Extract EVERYTHING.** Err on the side of too many facts. The Context Pack and Memo will decide what matters. Your job is completeness.

## What This Skill Does NOT Do

- ❌ External research or web searches (that's context-pack)
- ❌ Score frameworks, build TAM, or form investment opinions (that's investment-memo)

---

## Execution Flow

### Step 1: Gather Inputs

Check what's available:
- Pitch deck in uploads?
- Call notes / transcripts? (Check Granola by company + founder name)
- Data room files in uploads?
- Reference call notes?
- Enzo's initial take or IC questions?

If critical inputs are missing, ask before proceeding.

### Step 2: Process Inputs

Go through each input document systematically. Extract every fact. Tag it.

**Processing order:**
1. Data room files first (highest confidence)
2. Pitch deck
3. Founder call transcripts
4. Reference call transcripts

### Step 3: Build Contradictions & Flags

After processing all inputs:
- Cross-reference facts that contradict each other
- Flag all [METRIC] claims (link to raw data if available, flag as unverifiable if not)
- Flag all [MKT] and [COMP] claims as needing verification
- Note gaps in information

### Step 4: Save and Share

Save as `[Company]_Evidence_Ledger.md` in the deal folder.

Tell Enzo:
- Total evidence items extracted
- Number of contradictions found
- Number of [METRIC] claims that can be cross-checked vs. unverifiable
- Number of [MKT] and [COMP] claims flagged for verification
- Any critical gaps in inputs

**Enzo reviews and edits the Ledger before moving to Step 2 (context-pack) in a new session.**

---

## Skill Relationships

| Skill | Relationship |
|-------|-------------|
| **context-pack** | Consumes this output. Runs external research and validation. Separate session after Enzo reviews the Ledger. |
| **investment-memo** | Consumes both Ledger and Context Pack. Run in a third session. |
