---
name: context-pack
description: Verify claims, fill gaps, and surface market signals through hypothesis-driven research. This is Step 2 of 3 in the investment memo pipeline. Requires an Evidence Ledger as input. Triggers on "context pack", "verify the ledger", "run research", "check the claims", or when Enzo has a completed Evidence Ledger and wants to move to verification. Do NOT use for extracting facts (use evidence-ledger). Do NOT use for writing the memo (use investment-memo).
---

# Context Pack Generator

Validate the Evidence Ledger's flagged claims through external research, fill information gaps, and surface market signals the founder won't surface.

This is the **detective** step. The Ledger tells you exactly what needs checking. Your job is to go outside and come back with the truth.

## Architecture

This skill is a **thin orchestrator**. It does NOT contain its own research logic. Instead, it runs three existing atomic skills in sequence, then assembles and reconciles their outputs against the Evidence Ledger.

```
                        ┌─────────────────────┐
                        │   EVIDENCE LEDGER    │
                        │   (read as input)    │
                        └──────────┬──────────┘
                                   │
                    Extract claims needing verification
                                   │
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                    ▼
   ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐
   │ understand-       │ │ competitive-      │ │ market-           │
   │ industry          │ │ analysis          │ │ research          │
   │                   │ │                   │ │                   │
   │ Workflows, buyer  │ │ Competitor table, │ │ Bottoms-up TAM,   │
   │ logic, pain,      │ │ vulnerability,    │ │ segments, spend,  │
   │ procurement       │ │ threats, signals  │ │ tailwinds         │
   └────────┬─────────┘ └────────┬─────────┘ └────────┬─────────┘
            │                    │                     │
            └────────────────────┼─────────────────────┘
                                 │
                        ┌────────▼─────────┐
                        │   ASSEMBLY STEP   │
                        │                   │
                        │ • Claim-by-claim  │
                        │   reconciliation  │
                        │ • Team verif.     │
                        │ • Gap analysis    │
                        │ • Contradictions  │
                        └────────┬─────────┘
                                 │
                        ┌────────▼─────────┐
                        │  CONTEXT PACK     │
                        │  (output file)    │
                        └──────────────────┘
```

**Why this architecture:** The three atomic skills already contain best-in-class research logic, source priorities, and output structures. Duplicating that here means bugs get fixed in one place but not the other. Improvements to any atomic skill automatically flow into the Context Pack.

## Pipeline Position

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ EVIDENCE LEDGER │ ──▶ │  CONTEXT PACK    │ ──▶ │ INVESTMENT MEMO  │
│  (Step 1)       │     │  ★ YOU ARE HERE  │     │  (Step 3)        │
│  Done           │     │                  │     │  Separate session │
└─────────────────┘     └──────────────────┘     └──────────────────┘
```

**Input:** The `[Company]_Evidence_Ledger.md` file from Step 1.
**Output:** A `[Company]_Context_Pack.md` file that feeds into Step 3 in a separate session.

***

## Execution Flow

### Step 0: Read the Evidence Ledger

Read `[Company]_Evidence_Ledger.md` from the deal folder. Extract:

1. **Company name and website** — used as input for the 3 atomic skills
2. **All** **`[MKT]`** **claims** — market claims needing verification
3. **All** **`[COMP]`** **claims** — competitive claims needing verification
4. **All** **`[REG]`** **claims** — regulatory claims needing primary source verification
5. **All** **`[METRIC]`** **items marked UNVERIFIABLE (M3)** — the Ledger already cross-checked metrics with raw data; these are what it couldn't verify
6. **Contradictions & Flags** — anything the Ledger flagged that needs external context

Build a **research brief** from these extractions — a compact summary of:

* What the company does (from Ledger's Company & Product section)

* Who the customer is

* What the founder claims about market, competition, and regulation

* What specific claims need checking

This brief becomes the input prompt for each atomic skill.

### Step 1: Run understand-industry

**Read** `/mnt/.claude/skills/understand-industry/SKILL.md` and follow its full instructions.

**Input to the skill:**

* Industry/vertical from the Ledger (e.g., "How do K-12 school districts buy and use student information systems?")

* Company website for context

**What you're looking for:**

* Operational workflow that validates or challenges the founder's problem framing

* Buyer persona and procurement logic

* Current tech stack and pain points

* Budget and pricing sensitivity

**Save output as:** working notes in memory (not a separate file). You'll assemble it later.

### Step 2: Run competitive-analysis

**Read** `/mnt/.claude/skills/competitive-analysis/SKILL.md` and follow its full instructions.

**Input to the skill:**

* Company name and website

* The `[COMP]` claims from the Ledger as context (so the analysis directly addresses founder claims about competitors)

**What you're looking for:**

* Competitor table with revenue estimates, funding, differentiation

* Where the target wins and where it's vulnerable

* Non-obvious threats (platform risk, adjacent expansion, open-source)

* Incumbent signals (Glassdoor, G2, PE ownership dynamics)

**Save output as:** working notes in memory. You'll assemble it later.

### Step 3: Run market-research

**Read** `/mnt/.claude/skills/market-research/SKILL.md` and follow its full instructions.

**Input to the skill:**

* Market/vertical description from the Ledger

* The `[MKT]` claims from the Ledger as context (so sizing directly addresses founder TAM claims)

**What you're looking for:**

* Raw market sizing data points (customer counts, spend levels, growth rates)

* Customer segments with spending patterns

* Tailwinds and headwinds

* Comparable company valuations/metrics

**Important:** The market-research skill produces a bottoms-up TAM. Include the raw data points and segment counts in the Context Pack — the investment-memo skill (Step 3) will construct its own TAM from this data using its spend-grounded methodology.

**Save output as:** working notes in memory. You'll assemble it later.

### Step 4: Assembly — Claim Verification

Now reconcile the atomic skill outputs against the Ledger. Go claim by claim.

**For each** **`[MKT]`** **claim:**

* Match it against market-research and understand-industry findings

* Assessment: Confirmed / Partially confirmed (explain) / Challenged (counter-evidence) / Unverifiable

**For each** **`[COMP]`** **claim:**

* Match it against competitive-analysis findings

* Reminder: founders systematically understate competitor capabilities and overstate competitor weaknesses

* Assessment: Confirmed / Partially confirmed / Challenged / Unverifiable

**For each** **`[REG]`** **claim:**

* If not already covered by understand-industry, do targeted searches for primary sources (govt sites, regulatory bodies)

* Assessment: Confirmed / Partially confirmed / Challenged / Unverifiable

**For each UNVERIFIABLE** **`[METRIC]`** **(M3 flags):**

* Check if market-research or competitive-analysis surfaced benchmarks or comps that contextualize the metric

* If yes, note the comparison. If no, flag as still unverifiable.

### Step 5: Assembly — Team Verification

This is unique to the deal pipeline (not covered by atomic skills). Research:

* **LinkedIn vs. deck claims** — cross-reference founder backgrounds. Note discrepancies.

* **Prior company outcomes** — what happened to their previous companies? Exits, shutdowns, pivots.

* **Press mentions** — any public signal about the founders.

* **Backchannel signals** — from investor/operator networks if available.

Do 3-5 targeted searches per founder.

### Step 6: Assembly — Contradictions & Gaps

Review everything. Surface:

* New contradictions discovered during research (not in the Ledger)

* Claims that remain unverifiable after all research

* Gaps the Memo will need to acknowledge or work around

* Anything that surprised you — findings that diverge significantly from founder claims

### Step 7: Save and Share

Assemble the final file using the Context Pack Structure below. Save as `[Company]_Context_Pack.md` in the deal folder.

Tell Enzo:

* Total searches performed across all 3 skills + team verification

* Claims verified vs. challenged vs. unverifiable (counts)

* Key findings that surprised you (contradictions, competitor strengths, market data diverging from founder claims)

* Remaining gaps the Memo will need to acknowledge

* Any items that need Enzo's input before proceeding to Step 3

**Enzo reviews and edits the Context Pack before moving to Step 3 (investment-memo) in a new session.**

***

## Context Pack Structure

```markdown
# Context Pack: [Company Name]
Generated: [Date]
Evidence Ledger used: [filename]
Searches performed: [total count across all phases]

---

## 1. Industry Context

[Assembled from understand-industry output]

### How the Industry Works
- Operational workflow summary (the key steps, who does what, what breaks)
- Current tech stack and pain points

### Buyer Persona & Procurement
- Who buys, how they buy, budget source, buying cycle
- What kills deals, status quo bias, trigger events

### Budget & Pricing Sensitivity
- Current spend on this problem (including hidden costs)
- Realistic willingness to pay

---

## 2. Competitive Intelligence

[Assembled from competitive-analysis output]

### Competitor Table

| Category | Company | Revenue (est.) | Funding/Ownership | Differentiation | Threat Level |
|----------|---------|---------------|-------------------|-----------------|-------------|
| Incumbent | ... | ... | ... | ... | High/Med/Low |
| VC-backed direct | ... | ... | ... | ... | ... |
| Adjacent/platform | ... | ... | ... | ... | ... |
| Open-source/DIY | ... | ... | ... | ... | ... |

### Where Target Wins
[From competitive-analysis — validated or challenged against Ledger [COMP] claims]

### Where Target is Vulnerable
[From competitive-analysis]

### Incumbent Signals
- Glassdoor/Blind: [Morale, layoffs, innovation signals]
- G2/Capterra: [NPS, top complaints, feature gaps]
- Recent moves: [Acquisitions, launches, pricing changes]

### Non-Obvious Threats
[From competitive-analysis]

---

## 3. Market Data

[Assembled from market-research output]

### Key Market Data Points
- Market size data with sources
- Growth rates with sources
- Customer segment counts
- Spend levels by segment

### Tailwinds & Headwinds
[From market-research]

### Customer Segments
[From market-research — segment profiles, spending patterns, buying behavior]

NOTE: Raw market data goes here. The bottoms-up TAM construction and spend-grounded analysis happens in the Memo (Step 3).

---

## 4. Claim Verification

NOTE: The Ledger already cross-checked metrics against raw data room files. Only UNVERIFIABLE (M3) metrics appear here.

### Market Claims ([MKT])

For each [MKT] claim from the Ledger:

- **[E#] Founder claim:** "[Exact claim]" [Source]
- **Research finding:** [What market-research or understand-industry found]
- **Assessment:** Confirmed / Partially confirmed / Challenged / Unverifiable

### Competitive Claims ([COMP])

For each [COMP] claim from the Ledger:

- **[E#] Founder claim:** "[Exact claim]" [Source]
- **Research finding:** [What competitive-analysis found]
- **Assessment:** Confirmed / Partially confirmed / Challenged / Unverifiable

### Regulatory Claims ([REG])

For each [REG] claim from the Ledger:

- **[E#] Founder claim:** "[Exact claim]" [Source]
- **Primary source:** [Govt site, regulatory body, legal database]
- **Assessment:** Confirmed / Partially confirmed / Challenged / Unverifiable

### Unverifiable Metrics (M3 flags)

For each UNVERIFIABLE metric from the Ledger:

- **[E#] Claim:** "[metric]"
- **External benchmark:** [If found through market-research/competitive-analysis] or "No external benchmark found"

---

## 5. Team Verification

- [Founder 1]: LinkedIn vs deck claims, prior outcomes, press
- [Founder 2]: Same
- **Discrepancies:** [Any gaps between deck claims and external evidence]

---

## 6. Contradictions & Unresolved

- [C1] [Description — what the Ledger flagged vs what research found]
- [C2] [New contradiction discovered during research]
- [C3] [Unverifiable claim — no external source to check against]
- [C4] [Gap the Memo will need to acknowledge]
```

***

## Research Volume

The 3 atomic skills combined should produce:

* understand-industry: 5-10 searches

* competitive-analysis: 8-12 searches

* market-research: 8-15 searches

* Team verification: 3-5 searches per founder

**Total: 30-50+ searches per deal.** Roughly 1/3 industry + market context, 1/3 competitive intelligence, 1/3 claim verification + team. This is a floor, not a ceiling — complex deals with many competitors or unfamiliar industries need more.

***

## What This Skill Does NOT Do

* ❌ Extract facts from raw inputs or recalculate metrics (that's evidence-ledger)

* ❌ Score frameworks, build TAM, write narrative, or form investment opinions (that's investment-memo)

* ❌ Contain its own research logic — it delegates to atomic skills