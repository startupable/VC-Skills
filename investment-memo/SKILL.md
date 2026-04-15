---
name: investment-memo
description: Write the final investment memo for Reach Capital IC. This is Step 3 of 3 in the investment memo pipeline. Requires a completed Evidence Ledger and Context Pack as file inputs. Triggers on "write the investment memo", "write the memo for [Company]", or when Enzo has both a Ledger and Context Pack ready. Do NOT use for extracting facts (use evidence-ledger). Do NOT use for external research (use context-pack).
---

# Investment Memo Writer

Write the final investment document for Reach Capital IC. This is the **strategist** step — you synthesize the Evidence Ledger and Context Pack into a narrative memo that helps partners decide whether to invest.

## Pipeline Position

```
┌─────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ EVIDENCE LEDGER │ ──▶ │  CONTEXT PACK    │ ──▶ │ INVESTMENT MEMO  │
│  (Step 1)       │     │  (Step 2)        │     │  ★ YOU ARE HERE  │
│  Done           │     │  Done            │     │                  │
└─────────────────┘     └──────────────────┘     └──────────────────┘
```

**This skill does ONE thing:** read the Ledger + Context Pack files and write the narrative memo. No fact extraction. No external research. No metric recalculation. Those are already done.

## Inputs

**Required files (must exist before starting):**

1. `[Company]_Evidence_Ledger.md` — tagged facts with `[E#]` IDs, source tags, confidence tags, metric cross-checks, contradictions
2. `[Company]_Context_Pack.md` — claim verification results, competitive intelligence, market data, team verification, contradictions
3. `references/memo-calibration-examples.md` — voice and structure patterns from actual Reach memos

**Optional:**
- Enzo's initial take, IC questions, or specific framing requests

**If Ledger or Context Pack is missing:** Stop. Tell Enzo which file is missing and which skill to run first.

## Execution Flow

### Step 0: Read All Inputs

```
1. Read [Company]_Evidence_Ledger.md — this is your fact base
2. Read [Company]_Context_Pack.md — this is your verified research
3. Read references/memo-calibration-examples.md — this calibrates your voice
4. Read any additional context from Enzo (IC questions, framing preferences)
```

**The enforcement mechanism:** Every factual claim in the Memo must trace back to the Ledger `[E#]` or Context Pack. If you find yourself writing something that isn't in either file, stop — it's either fabricated or editorializing.

### Step 1: Plan the Memo

Before writing, identify:
- What's the thesis? (from Ledger + Context Pack, what makes this investable?)
- What's the deal origin? (how did this reach Reach?)
- What's the core risk? (from contradictions and unresolved items)
- What section order best serves this deal? (default order below, but reorder if needed — e.g., if team IS the thesis, lead with Team after Summary)

### Step 2: Write the Memo

Follow the structure below. Footnote every factual claim with `[E#]`.

### Step 3: Save and Share

Save as `[Company]_Full_Investment_Memo.md` in the deal folder. Tell Enzo:
- Word count
- Key editorial choices (what you emphasized, what you deprioritized, why)
- Any gaps where Ledger/Context Pack didn't have enough to write with confidence

---

## Memo Structure

### HEADER

```markdown
# Investment Memo — [Company Name]
**[Month Year]**
**Team:** [First names + last initials]

## Diligence Materials
- [Pitch Deck](link)
- [Product Demo](link)
- [Customer References](link)
- [Founder References](link)
- [Data Room](link)
```

---

### SUMMARY (300-500 words, narrative prose)

The opening case. Partners read this first, sometimes only this.

Must accomplish, in this order:

1. **What the company does** — 1-2 sentences, product-first, not problem-first
2. **Deal origin story** — How this deal reached Reach. Who sourced it, prior touchpoints, why it felt worth pursuing. This is the narrative hook — "why are we here" before the thesis.
3. **The thesis** — Why Reach should back this team on this problem. State it clearly: "Our thesis is that..."
4. **The team** (2-3 sentences) — Why these specific founders are the right people. Name them, surface the thread line from their background, anchor with a reference signal. This is NOT a bio — it's the founder-market fit argument compressed. Examples: "Bem is led by repeat founder Antonio Bustamante, who previously built Kite and Silo." / "We believe that the combination of a technically sophisticated and narratively adept team... make GPTZero the right company to back."
5. **The wedge** — specific entry point and why it works
6. **Timing** — what changed that makes this possible now
7. **Core risk** — the one thing that has to be true, named honestly
8. **Preview the recommendation** — are we in or out, at what terms

**Voice calibration:**
- ✅ Lead with the company and product, not the market or problem
- ✅ Name specific customers, metrics, reference signals
- ✅ State the bet clearly: "Our thesis is that..."
- ❌ "We are excited about..." / "The market represents a compelling..."
- ❌ Generic framing without specifics

Footnote claims: `[E#]`

---

### RECOMMENDATIONS & TERMS

**A. Round Dynamics** (1-2 paragraphs, narrative prose)

Describe the round as a story, not a data dump:
- Total raise, instrument, valuation/cap `[E#]`
- Lead investor: who, check size, track record `[E#]`
- If no lead: say so, describe structure and commitments `[E#]`
- How much spoken for, allocation remaining `[E#]`
- Competitive dynamics: who else is interested, leverage or tension `[E#]`

**B. Recommendation**

**We recommend [action] at [valuation/cap], targeting [X%] ownership with a [$X] check.** `[E#]`

**C. Cap Table Summary** (brief)

Quick snapshot — instrument, major investors, founder splits, option pool. Not a full waterfall.

**D. Key Milestones for This Capital** (3-5 specific, measurable items)

- [Milestone 1] `[E#]`
- [Milestone 2] `[E#]`
- [Milestone 3] `[E#]`

---

### TEAM (400-800 words, narrative prose with embedded bios)

**A. Opening take** (2-3 sentences)

Lead with your honest assessment of team quality and character. What stands out?

Examples:
- "This is a young (early 20s), first-time founding team with a unique combination of technical expertise and storytelling ability, influenced by Edward's background in journalism."
- "The Scout team stands out for their high technical expertise, thoughtfulness, and fresh perspective on the SIS market."

**B. Founder narratives** (2-3 paragraphs per founder, prose — not bullet lists)

For each founder, tell the STORY. Build a narrative arc:

1. **The thread line** — What connects their experiences? What pattern emerges when you read their career as a story? (Edward: journalism + CS + Princeton NLP lab → detection as a mission. Antonio: healthcare startup → Kite → Silo → Bem = decade-long frustration with production AI.)

2. **The superpower** — Show through specific actions, not labels:
   - ✅ "He organized the UK's largest student startup conference — 2,500 attendees, put together in two months, with ~$200K raised from VCs"
   - ✅ "He turned down jobs at Microsoft and the New York Times to start GPTZero"
   - ❌ "Antonio is a rare mix of technical depth and practical execution"

3. **Formative failures or pivots** — Prior shutdowns, pivots, lessons learned. (Paul's Coleap failure directly shaped Solea's product philosophy.)

4. **Reference signals woven in** — Don't isolate references into a separate paragraph. Weave them into the narrative: "Investors describe Antonio as... `[Ref:Name]`" / "Michel Mosse shared: 'Chris is very solid as a founder' `[Ref:Name]`"

5. **Founder-market fit** — Why do they care about THIS problem? Show through biography, not assertion.

**C. Founding relationship** (2-4 sentences)

How founders know each other, how the partnership works, how the dynamic evolved.

**D. Team composition & gaps** (1 paragraph + optional engineer list)

Current team size, functional breakdown, key hires, engineering quality signals, and **the honest gap** — biggest team risk, named directly.

For engineering-heavy companies:
```markdown
* [Name] ([Role]): [1-line background] [E#]
* [Name] ([Role]): [1-line background] [E#]
```

**Rules:**
- Cross-reference LinkedIn vs. deck using Context Pack team verification — note discrepancies
- Founder-market fit must be SHOWN through narrative, not stated as conclusion
- Reference quotes stay in quotes with attribution `[Ref:Name]`
- Write in prose paragraphs, not bullet lists

---

### MARKET & GTM (500-1000 words)

Prose + tables. Assume IC does not know this market.

**A. How the industry works** (2-3 paragraphs)

Walk through the operational workflow step by step. Where does the pain sit? What breaks? Regulatory complexity with specific examples. Use customer quotes and reference data from the Ledger. Pull from the Context Pack's industry context section.

**B. Market structure and ICP** (1-2 paragraphs)

Introduce the market segments that flow into the TAM. Who buys? Current workflow? Current spend? How do they buy?

This section sets up section C — the reader should understand the segments before seeing the sizing table.

**C. Bottoms-Up TAM** (table + assumptions)

Build from what customers actually spend today — legacy software, labor, workarounds. ACV is justified as a capture rate of that current spend, not pulled from air.

| Segment | # Customers | Current Spend | ACV (est.) | TAM |
|---------|------------|---------------|------------|-----|
| Wedge ICP | X | $Y/yr on [what] | $Z | $A |
| Adjacent 1 | X | $Y/yr on [what] | $Z | $B |
| Aspirational | X | $Y/yr on [what] | $Z | $C |

> **Key Assumptions:** [2-4, each sourced or flagged] `[E#]`

**Rules:**
- If founder provided TAM, present BOTH theirs and yours. Note divergence.
- Separate wedge (12-18 months) from aspirational (3+ years).
- Every number sourced or assumption-flagged.
- Use Context Pack's market data section as your raw inputs.

**D. Why Now** (1-2 paragraphs)

What changed in the world that makes this company possible or necessary TODAY but not 3 years ago? Prioritize:

1. **Technology shift** — what capability became available, cost/performance implications
2. **Regulatory/policy change** — new mandate, compliance requirement, market structure change
3. **Behavioral/cultural shift** — buyer readiness, labor market conditions, adoption patterns

⚠️ **Competitor weakness is NOT a Why Now.** "The incumbent is struggling" belongs in section F. Test: if you removed all competitor references, does your Why Now still stand?

**E. GTM and expansion** (1-2 paragraphs)

Sales motion today. Land-and-expand hypothesis (labeled as hypothesis).

**F. Competitive landscape** (table + prose)

| Category | Company | Revenue (est.) | Funding/Ownership | Differentiation | Threat Level |
|----------|---------|---------------|-------------------|-----------------|-------------|
| Incumbent | ... | ... | ... | ... | High/Med/Low |
| VC-backed direct | ... | ... | ... | ... | ... |
| Adjacent/platform | ... | ... | ... | ... | ... |
| Open-source/DIY | ... | ... | ... | ... | ... |

Use Context Pack's competitive intelligence section as your source.

**After the table, address in prose:**

1. **Where the company wins** (1 paragraph) — Validate or challenge the claimed differentiator against Context Pack findings.

2. **Incumbent vulnerability analysis** (1 paragraph, when relevant) — PE ownership → cost-cutting → talent flight → innovation freeze. Use Glassdoor, Blind, layoff signals from Context Pack.

---

### PRODUCT (500-1,000 words, prose)

Depth scales with product complexity: ~500 words for simple products, 800-1,000 for complex ones.

Write in continuous prose. Do NOT use the guidance below as section headers — weave into a narrative.

**Start with how it works.** Walk through as a user would experience it. Anchor with real customer examples. For complex products, break architecture into layers. For products in complex workflows, map the workflow step-by-step then show where the product intervenes.

**Then make the 10x argument.** Before/after story. What did the customer do before? What now? How much time, money, or pain eliminated? Use customer quotes. Quantify when possible.

**Address what's hard to build and hard to leave.** Technical challenge for replication? Stickiness — data gravity, workflow integration, switching cost?

**Separate live from roadmap.** Never present roadmap as shipped. If roadmap items are relevant, state in a short paragraph at the end, clearly labeled.

---

### DEFENSIBILITY — 8 MOATS (Gokul Rajaram framework)

Score each moat 0 / 0.5 / 1 based on evidence from the Ledger and Context Pack. Most early-stage companies score 2-4 out of 8.

| Moat | Score | Evidence |
|------|-------|----------|
| Data | 0/0.5/1 | [Does usage generate proprietary data that improves the product?] |
| Workflow | 0/0.5/1 | [Is the product embedded in a daily workflow that's painful to rip out?] |
| Regulatory | 0/0.5/1 | [Does compliance create a barrier to entry?] |
| Distribution | 0/0.5/1 | [GTM advantage competitors can't easily replicate?] |
| Ecosystem | 0/0.5/1 | [Third parties building on or integrating with this product?] |
| Network | 0/0.5/1 | [Product gets more valuable as more users join?] |
| Physical | 0/0.5/1 | [Hardware, logistics, or physical presence as barrier?] |
| Scale | 0/0.5/1 | [Scale drives cost advantages or performance improvements?] |
| **TOTAL** | **X/8** | |

> **#1 moat-building priority for next 12 months:** [Specific recommendation]

1-2 sentences connecting score to investment thesis.

---

### TRACTION (300-600 words)

**A. Business Model** (1-2 paragraphs)

How the company makes money. Pricing model, tier structure with named customer examples at each tier, expected evolution.

**B. Traction Metrics**

Write as a narrative about commercial momentum — not a data dump with bold labels.

Open with the headline number (ARR, revenue run rate). Build outward: customers (logos, ACV range, concentration), growth (expansion, NDR, churn), pipeline. Weave in unit economics where available.

Customer reference signals belong inside the narrative, not separate. Drop quotes where relevant.

**Data to cover** (weave in, don't list): revenue, customers, engagement/retention, pipeline, unit economics, customer reference signals `[Ref:Name]`.

Frame honestly. Data room numbers (from Ledger) take precedence over deck claims. Note discrepancies.

---

### KEY RISKS & MITIGATIONS (table)

| Risk | Mitigation |
|------|-----------|
| [Specific risk — market/TAM] | [Concrete mitigation] |
| [Specific risk — execution/team] | [Concrete mitigation] |
| [Specific risk — competitive] | [Concrete mitigation] |
| [Specific risk — product/technical] | [Concrete mitigation] |
| [Specific risk — from references] | [Concrete mitigation] |

4-8 rows. Each risk specific and named (not "competition" but "Reducto raised $25M targeting same segment"). Mitigations concrete.

---

### APPENDIX (variable, on-demand)

Deeper analysis that supports but doesn't belong in the main narrative:
- State-by-state / geography expansion analysis
- Detailed competitive deep dive
- Financial model review
- Customer reference summaries
- Technical architecture notes
- Comparable company analysis

Not every memo needs an appendix.

---

## Writing Guidelines

### Voice

Direct, analytical, no fluff. Write like a strategist talking to partners. These memos are read by busy investors who want the bet in 5 minutes and the nuance in 15.

### Narrative vs. Editorializing

The memo is an authored document with a point of view — that's what makes it useful to IC.

**Where conviction belongs:**
- **Summary:** The investment thesis. "We believe..." / "Our thesis is..."
- **Key Risks:** Honest concerns. Name the bet.
- **Team opening take:** 2-3 sentence assessment of team quality.
- **Framing throughout:** The author's conviction shows in WHAT they emphasize, HOW they sequence, WHICH evidence they foreground.

**The line between good framing and bad editorializing:**
- ✅ Presenting Antonio's three prior companies in sequence, letting the pattern speak
- ❌ "Antonio is a case study in adaptability"
- ✅ Noting PowerSchool laid off 5% under PE ownership, then describing Scout's approach
- ❌ "This macro positioning is what opens C-suite doors"
- ✅ "Andy from Uncork stack-ranks Antonio and Upal as among the top 10% of seed-stage founders" `[Ref:Name]`
- ❌ "This is a rare combination"

**The test:** If a sentence tells the reader what to conclude — cut it. If it presents evidence that leads the reader to a conclusion — that's good writing.

### Length

- Total (excl. appendix): 2,500-4,000 words (~5-8 pages)
- No section exceeds 800 words

### Evidence Discipline

| Type | Presentation | Citation |
|------|-------------|----------|
| Data room fact | State directly | `[E#]` |
| Founder call | State directly or "The founder reports..." | `[E#]` |
| Reference call | "Dan Carroll noted..." | `[E#]` |
| Web research | State directly | `[E#]` (source in ledger) |
| Your hypothesis | "We believe..." / "Our thesis is..." | Label as hypothesis |
| Assumption | State then flag | `[Assumption: reason]` |
| Unverified claim | "The company claims..." | `[E#]` + note unverified |

### Prohibited Language

Never use:
- "Exciting opportunity" / "We are excited" / "compelling"
- "Disruptive" / "game-changing" / "revolutionary"
- "Best-in-class" (unless quoting a benchmark with source)
- "Leverage synergies" or consultant-speak
- "Move fast and break things" or startup platitudes
- "It depends" / "it's nuanced" (take a position)
- "Rapidly growing market" (give the growth rate)
- "Strong team" without specifics
- "Modern UI" as a differentiator without evidence
- "AI-powered" as a feature (explain what the AI does)
- "Tailwinds" without naming the specific force
- "Massive opportunity" / "huge market" (give the TAM number)

### What NOT to Do

- Never fabricate any detail — not just metrics. If a workflow description or business mechanic is not in a source, do not write it.
- Never present roadmap as shipped capability
- Never reuse facts from calibration examples
- Never write generic risks
- Never present founder claims as verified without Context Pack confirmation
- Never mix reference quotes with founder claims

---

## Reference Files

**CRITICAL:** Read the calibration file BEFORE writing any memo section.

| File | When to Read | What It Calibrates |
|------|-------------|-------------------|
| `references/memo-calibration-examples.md` | **Before writing ANY section** | Voice, depth, and structure patterns for ALL sections extracted from actual Reach memos |

---

## What This Skill Does NOT Do

- ❌ Extract or tag facts from raw inputs (that's evidence-ledger)
- ❌ Run external research or verify claims (that's context-pack)
- ❌ Recalculate metrics from data room files (that's evidence-ledger)
- ❌ Build competitive intelligence from scratch (that's context-pack → competitive-analysis)
- ❌ Size the market from scratch (that's context-pack → market-research)

This skill reads finished files and writes the narrative. That's its only job.
