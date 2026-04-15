---
name: pre-ic-memo
description: Generate 4-page Pre-IC investment memos for Reach Capital from a pitch deck and/or call notes. Relies heavily on external research to fill gaps. Includes Gokul Rajaram's 8 Moats defensibility scoring. Triggers on "pre-IC memo", "pre IC", "write a pre-IC", "investment brief", or when asked to evaluate a startup with a deck and limited information. Do NOT use for First Looks (use first-look skill) or full investment memos (use full-investment-memo skill).
---

# Pre-IC Investment Memo Generator

Generate 4-page research-backed investment briefs from a pitch deck + 1 call. Answers: "Is this worth full diligence?"

## When to Use

* After 1 call + pitch deck. Little proprietary data.

* Before the team decides whether to go deeper.

* NOT for initial screening (use first-look) or post-diligence memos (use full-investment-memo).

## Input Requirements

**Required (at least one):**

* Pitch deck (.pptx or .pdf)

* Call notes, transcript, or meeting summary

**Optional:**

* Enzo's initial take / interest bullets

* Deck link (to reference in output)

* Specific questions from the team

If only a deck is provided, proceed — research fills the gaps.

***

## Phase 1: Extract Facts from Inputs

### Pitch Deck Extraction

If .pptx:

```
pip install python-pptx --break-system-packages -q

```

```
from pptx import Presentation
prs = Presentation('/mnt/user-data/uploads/[deck].pptx')
for i, slide in enumerate(prs.slides):
    print(f"\n--- Slide {i+1} ---")
    for shape in slide.shapes:
        if shape.has_text_frame:
            for para in shape.text_frame.paragraphs:
                print(para.text)

```

If .pdf: use the pdf skill to extract text.

### Build a Fact Sheet

Extract and organize everything claimed in the inputs:

```
FACT SHEET (from inputs)
========================
Company: [Name]
Website: [URL]
What they do: [1-2 sentences]

TEAM (as claimed)
- [Founder 1]: [Title, background as stated]
- [Founder 2]: [Title, background as stated]
- Team size: [X]

MARKET (as claimed)
- ICP: [Who they sell to]
- Market size claim: [What they say]
- Why now: [Their argument]

PRODUCT (as claimed)
- What's live: [Features/capabilities]
- What's roadmap: [Planned features]
- Key differentiator: [Their claim]

TRACTION (as claimed)
- Revenue/ARR: [X]
- Customers: [X]
- Growth: [X]
- Other metrics: [engagement, retention, etc.]

TERMS (as claimed)
- Raising: [$X]
- Instrument: [SAFE/priced round]
- Valuation/cap: [$X]
- Committed: [$X from whom]

GAPS (what's missing from inputs)
- [List everything not covered]

```

***

## Phase 2: Research (5-10 min automated)

Research fills what the deck doesn't cover and pressure-tests what it claims. Use web search, PitchBook (if available via MCP), and any connected tools.

### Research Checklist

**Founders (always research):**

* \[ ] LinkedIn profiles: prior companies, roles, tenure, education

* \[ ] Prior company outcomes: exits, shutdowns, funding raised

* \[ ] Domain expertise signals: did they work in this industry?

* \[ ] Press mentions, talks, writing

**Funding (always research):**

* \[ ] Prior rounds: amounts, investors, dates (PitchBook or Crunchbase)

* \[ ] Investor quality: who backed them and why it matters

* \[ ] Cap table context if available

**Market (always research):**

* \[ ] Market size data: TAM estimates from reports, public filings, industry sources

* \[ ] Industry structure: who are the buyers, what do they spend today

* \[ ] Regulatory context if applicable

* \[ ] Macro tailwinds/headwinds

**Competitors (always research):**

* \[ ] Direct competitors: funding, traction, positioning

* \[ ] Incumbent financials if public (revenue, margins, growth)

* \[ ] Adjacent players that could pivot into the space

* \[ ] Glassdoor/employee signals for incumbents (morale, layoffs, innovation pace)

**Validation (if time permits):**

* \[ ] Customer reviews (G2, Capterra) for competitors

* \[ ] Press coverage of the space

* \[ ] Any analyst reports or market maps

### Research Boundaries

* **Do:** 5-8 targeted web searches. Verify founder claims. Get competitor context.

* **Don't:** Spend 20 minutes. This is pre-IC, not diligence.

* **Flag:** If something critical can't be verified, put it in Open Questions.

### Cite Sources

Every research-sourced data point must include: `[Source: Name — URL]` If speculative, flag: `Assumption: [what and why]`

***

## Phase 3: Synthesize the Memo

### Output Format: Markdown (default)

Draft in markdown in chat. On request, generate .docx (see Phase 4).

### Section Order and Depth

***

#### HEADER (compact, top of document)

```
**COMPANY:** [Name] ([URL])
**DATE:** [MM/DD/YY]
**STAGE:** [Pre-Seed / Seed / Series A]
**RAISING:** [$XM] on [instrument] at [cap/valuation]
**HQ:** [City, State / Country]
**REACH CONTACT:** [Initials]
**FOUNDERS:** [Name (CEO), Name (CTO)] — [1-line on backgrounds]

```

No table. Just clean metadata.

***

#### 1. THE BET (3-5 sentences, \~100 words)

One paragraph that answers: What is this company, what's the wedge, why now, and why should Reach care?

**Rules:**

* Lead with what the company does and for whom, not with the problem

* Include the timing insight (why this is newly possible)

* End with why Reach specifically should care (impact, portfolio fit, thesis)

* Be opinionated. This is your pitch to IC in 30 seconds.

**Tone calibration (from Reach memos):**

* Good: "Scout is building a modern SIS for independent and microschools, starting with California's independent study programs as a wedge into the broader $2B K12 SIS market."

* Bad: "The K12 SIS market is large and growing, presenting an interesting opportunity for disruption."

***

#### 2. TEAM (\~200 words, prose)

Who the founders are, why they're the right people, what's the risk.

**Must include:**

* Founder backgrounds from research (prior companies, roles, outcomes — not just what the deck claims)

* Founder-market fit signal: why do they care about this problem?

* Team composition: size, key hires, what functions exist

* The honest team gap: what's missing that matters most?

**Rules:**

* Use researched backgrounds, not just deck claims. If LinkedIn shows something different from the deck, note the discrepancy.

* "Strong technical team" is not a finding. "Both founders built workflow automation at Series B startups (Slash, TodayTix) before entering edtech" is.

***

#### 3. MARKET & TIMING (\~250 words + sizing table)

**Must include:**

A. **ICP and pain** (2-3 sentences): Who is the customer today? What's the concrete workflow or pain point? Make the reader feel it.

B. **Why now** (2-3 sentences): What changed — regulatory, technological, behavioral — that creates the opening? Be specific, not generic.

C. **Sizing table:**

| Segment                    | # of Customers | ACV (est.) | TAM |
| :------------------------- | :------------- | :--------- | :-- |
| Wedge ICP                  | X              | $Y         | $Z  |
| Adjacent segment           | X              | $Y         | $Z  |
| Full market (aspirational) | X              | $Y         | $Z  |

> **Assumptions:** \[List 2-3 key assumptions. Every number must be sourced or flagged as an assumption.]

D. **Expansion path** (2-3 sentences): How does the wedge grow? Label as hypothesis. Be honest about what has to be true.

***

#### 4. PRODUCT & DIFFERENTIATION (\~150 words, prose)

**Must include:**

* What's live today vs. roadmap (explicit separation)

* The core "10x better" argument — why would someone switch?

* 2-3 most relevant competitors and why this company can win

* If applicable: what's technically hard about building this?

**Rules:**

* Don't list features. Explain the workflow change.

* Don't say "modern UI" as a differentiator unless there's specific evidence.

* Name the strongest competitor and explain why the company can still win.

***

#### 5. TRACTION & PROOF POINTS (\~100 words)

**Must include:**

* Whatever exists: ARR, pilots, LOIs, waitlist, engagement proxy

* Frame honestly: "2 paid pilots at $15K" is fine if that's the reality

* If pre-revenue: what's the strongest qualitative signal?

**Rules:**

* Never inflate. If it's early, say so.

* If the deck claims metrics you can't verify, flag it.

***

#### 6. DEFENSIBILITY HYPOTHESIS — 8 Moats (table + 1 paragraph)

Score using Gokul Rajaram's 8 Moats Framework, adapted for early-stage.

**Scoring for early-stage companies:**

* **1** = Structural advantage already observable or architecturally locked in

* **0.5** = Credible path given current product/positioning decisions

* **0** = No path visible, or requires winning the market first to exist

**Key rule:** Score what they have or could realistically have based on current architecture and positioning. NOT what happens if they 10x. If a moat requires massive scale to kick in, it's a 0.

**Note:** Data Moat and Workflow Moat compound together for systems of record. The workflow creates the data, and the data makes the workflow harder to leave. Score them independently but note the interaction.

| Moat             | Score       | Hypothesis                                                                      |
| :--------------- | :---------- | :------------------------------------------------------------------------------ |
| **Data**         | 0 / 0.5 / 1 | Proprietary, compounding data asset? Does it get better with every interaction? |
| **Workflow**     | 0 / 0.5 / 1 | Depth of operational embedding? System of record, or lightweight layer?         |
| **Regulatory**   | 0 / 0.5 / 1 | Licenses, capital requirements, compliance barriers?                            |
| **Distribution** | 0 / 0.5 / 1 | Proprietary or exclusive distribution channels?                                 |
| **Ecosystem**    | 0 / 0.5 / 1 | Third-party developers/apps built on the platform?                              |
| **Network**      | 0 / 0.5 / 1 | Network effects, marketplace density, liquidity?                                |
| **Physical**     | 0 / 0.5 / 1 | Atoms, hardware, real-world infrastructure?                                     |
| **Scale**        | 0 / 0.5 / 1 | Structural cost advantage from scale? (Pure SaaS almost never qualifies)        |
| **TOTAL**        | **X/8**     | **4+ = Strong · 2-3 = Weak · 0-1 = No durable moat**                            |

**After the table, one paragraph:**

> "The #1 moat-building priority for the next 12 months is \_\_\_."

Explain which moat is most realistic to build given current positioning, and what the company would need to do.

**AI-era adjustments to apply:**

* Scale moat is dead for pure software companies

* Brand is not a moat for B2B

* For early-stage: focus on data compounding + workflow depth

* Vertical companies must show ambition to own full stack, not just one function

***

#### 7. WHAT COULD GO RIGHT / WHAT COULD GO WRONG

Two opposing assessments of the opportunity. Forces the memo to make the bull AND bear case explicit.

**What Could Go Right (3 bullets max)**

The optimistic scenario. What happens if the key assumptions hold and execution goes well?

* \[Bullet 1]: The strongest upside case — what's the "this could be massive" argument?

* \[Bullet 2]: A second-order positive — a tailwind, flywheel, or expansion path that compounds

* \[Bullet 3]: The non-obvious upside — something most people would miss

**What Could Go Wrong (3 bullets max)**

The pessimistic scenario. What breaks, and why?

* \[Bullet 1]: The existential risk — the one thing that kills the company

* \[Bullet 2]: The execution risk — the hardest thing they need to pull off

* \[Bullet 3]: The market/timing risk — what if the thesis is wrong?

**Rules:**

* Be specific. "Competition" is not a risk. "Reducto just raised $25M and is targeting the same mid-market logistics segment" is.

* Be specific on the upside too. "Big market" is not a bull case. "If they crack state-by-state compliance replication, the wedge TAM jumps from $12M to $200M" is.

* Each bullet should be 1-2 sentences max. Concise and sharp.

* The optimistic case should NOT just be "the opposite of the risks." It should articulate genuine upside mechanics.

***

#### 8. TERMS & RECOMMENDATION (one line)

One sentence. Examples:

* "We recommend a $2M check on a $20M post-money cap SAFE for \~10% ownership."

* "Continue diligence: need to validate state-by-state expansion thesis and speak with 2-3 customers."

* "Pass: TAM is too constrained without international expansion we don't see as credible."

* "Keep warm: revisit when they have 3+ paying customers outside California."

***

#### 9. OPEN QUESTIONS FOR IC (5-8 questions)

Specific questions that came out of the analysis. These should be things that could swing the decision and can't be answered without more diligence.

**Rules:**

* Frame as questions, not concerns

* Be specific enough that someone could go answer them

**Bad:** "Is the TAM big enough?" **Good:** "Can they expand from California ISPs to other states' compliance frameworks, or is the regulatory structure too state-specific to replicate?"

**Bad:** "Is the team strong enough?" **Good:** "Noah has no education experience — how has Butte County's deputy superintendent reacted to working with an outsider team, and what did that validation look like?"

***

## Phase 4: Generate .docx (on request)

When the user asks to convert to docx, generate using docx-js (the `docx` npm package).

### Setup

```
npm install -g docx 2>/dev/null

```

### Document Structure

Use the docx skill's approach (see /mnt/skills/public/docx/SKILL.md) to create a portrait US Letter document with:

* **Font:** Arial 11pt body, Arial 14pt bold for section headers

* **Margins:** 1 inch all sides

* **Page size:** US Letter (12240 x 15840 DXA)

* **Headers:** Bold, slightly larger, with spacing

* **Tables:** Light gray borders, proper cell padding, DXA widths

* **Moats table:** Shaded header row, 3 columns

Write a Node.js script that creates the document:

```
node /home/claude/generate_pre_ic.js
cp /home/claude/[CompanyName]_Pre_IC_Memo.docx /mnt/user-data/outputs/

```

***

## Writing Guidelines

### Voice

* Direct, analytical, no fluff. Write like a strategist talking to a partner.

* Use specific numbers over vague qualifiers.

* Lead with the strongest point in each section.

* Be opinionated but honest — don't sell, don't hedge everything.

### Formatting

* Prose-first. Bullets only for: terms, sizing tables, moats table, what could go right/wrong, open questions.

* No section should exceed 400 words.

* Total memo: \~2500-3000 words (\~4 pages portrait).

### Evidence Discipline

* Facts from the deck: state as claims ("The company reports $270K ARR")

* Facts from research: cite source ("PowerSchool was acquired for $5.6B \[Source: Vista Equity — URL]")

* Hypotheses: label explicitly ("Assumption: independent study compliance workflows are similar enough across states to replicate the CA playbook")

* Unknowns: flag and put in Open Questions

### What NOT to Do

* Don't reuse facts from example memos in this skill file

* Don't fabricate metrics, customer names, or quotes

* Don't present roadmap features as shipped capabilities

* Don't write generic risks ("competitive market is crowded")

* Don't write more than one line for Terms & Recommendation

***

## Quick Reference: How This Differs from Other Skills

| Dimension    | First Look        | **Pre-IC Memo**               | Full Memo                 |
| :----------- | :---------------- | :---------------------------- | :------------------------ |
| Length       | 1 page landscape  | **4 pages portrait**          | 5-15 pages                |
| When         | Initial screening | **After 1 call + deck**       | After full diligence      |
| Research     | 2-3 min           | **5-10 min automated**        | Manual deep diligence     |
| Thesis depth | 1 line            | **3-5 sentence "Bet"**        | Full narrative            |
| Market       | 1 bullet          | **Sizing table + prose**      | Bottoms-up TAM            |
| Team         | 1 bullet          | **1/2 page with backgrounds** | 1-2 pages + references    |
| Moats        | Not included      | **8 Moats scored**            | Full competitive analysis |
| Risks        | 1-2 bullets       | **Go right / go wrong (3+3)** | 6-12 with mitigations     |
| Decision     | "Worth a call?"   | **"Worth full diligence?"**   | "Should we invest?"       |