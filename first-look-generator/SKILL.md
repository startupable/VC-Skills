---
name: first-look
description: Generate First Look investment memos for Reach Capital. Use when given raw startup data (pitch deck, call notes, founder meeting notes) to create a concise 1-page summary for the investment team. Triggers on phrases like "first look", "create a first look", "investment memo", or when asked to summarize a startup opportunity for diligence.
---

# First Look Generator

Generate concise 1-page investment memos from raw startup data (pitch decks, call notes, transcripts) using the official Reach Capital template.

## Purpose

First Looks communicate in 2-3 minutes why an investor is interested in taking a deeper look at a company. They enable the Reach Capital team to quickly assess investment opportunities.

## Input Requirements

**Required:**
- Raw data: pitch deck, call notes, transcript, or founder meeting notes

**Optional but helpful:**
- 1-2 bullets on why the investor is interested
- Deck link (to include in output)

If interest bullets aren't provided, infer from the data and complete the template anyway.

---

## Step 1: Research to Fill Gaps

**Before generating the First Look, conduct lightweight research to complete missing context.**

Raw inputs (call notes, decks) often lack easily verifiable details. Use web search and PitchBook to fill gaps on:

### Company Details
| Field | Research If Missing |
|-------|---------------------|
| HQ location | Search "[Company] headquarters" or check LinkedIn company page |
| Stage | Infer from raise amount or search "[Company] funding round" |
| Prior funding | PitchBook or search "[Company] crunchbase funding" |
| Notable investors | PitchBook or search "[Company] investors" |
| Competitors | Search "[Company] competitors" or "[space] startups" |

### Founder/Team Details
| Field | Research If Missing |
|-------|---------------------|
| Founder full names | LinkedIn or company website |
| Prior companies | LinkedIn profiles, search "[Founder name] founder" |
| Prior exits | Search "[Founder name] acquisition" or "[Prior company] exit" |
| Domain expertise | LinkedIn, prior roles at relevant companies |
| Education (if notable) | LinkedIn (only include if MIT/Stanford/etc. or directly relevant) |

### Product & Market
| Field | Research If Missing |
|-------|---------------------|
| Product description | Company website, recent press coverage |
| Market size | Search "[market] TAM" or "[industry] market size" |
| Key competitors | Search "[Company] vs" or "[space] market map" |

### Research Tools Priority
1. **PitchBook** (if available via MCP) - Best for funding history, investors, company details
2. **Web search** - For founder backgrounds, press coverage, market context
3. **LinkedIn** (via browser) - For founder details if needed

### Research Boundaries
- **Do research:** Company basics, founder backgrounds, funding history, market context
- **Don't over-research:** This is a First Look, not full diligence. 2-3 minutes of research max.
- **Flag unknowns:** If something critical is unclear after quick research, note it in QUESTIONS FOR REACH TEAM

### Research Output Format

After research, organize findings before writing:

```
RESEARCH SUMMARY
================
Company: [Name]
HQ: [City, State]
Founded: [Year]
Website: [URL]

TEAM
- [Founder 1]: [Title]. Previously [Prior company/role]. [Notable credential].
- [Founder 2]: [Title]. Previously [Prior company/role].

FUNDING HISTORY
- [Date]: [Stage] - $[Amount] from [Investors]
- [Date]: [Stage] - $[Amount] from [Investors]
Current raise: $[Amount] at [Stage]

KEY CONTEXT (from research)
- [Any relevant market/competitive context discovered]
- [Any notable press or validation]

GAPS REMAINING
- [List anything still unclear for QUESTIONS section]
```

---

## Step 2: Generate the First Look

### Implementation: Template-Based Approach

**CRITICAL: Always use the template file, never generate from scratch.**

The First Look template is located at:
```
/mnt/skills/user/first-look/References/First_Look_Template.docx
```

### Process

1. **Copy the template:**
```bash
cp /mnt/skills/user/first-look/References/First_Look_Template.docx /home/claude/[CompanyName]_First_Look.docx
```

2. **Unpack the template:**
```bash
cd /mnt/skills/public/docx/scripts
python unpack.py /home/claude/[CompanyName]_First_Look.docx /home/claude/first_look_unpacked/
```

3. **Edit the XML directly** using string replacements on `/home/claude/first_look_unpacked/word/document.xml`

4. **Pack it back:**
```bash
python pack.py /home/claude/first_look_unpacked/ /mnt/user-data/outputs/[CompanyName]_First_Look.docx --original /mnt/skills/user/first-look/References/First_Look_Template.docx
```

### Template Placeholders to Replace

| Field | Find Pattern | Replace With |
|-------|--------------|--------------|
| DATE | `DATE: 9/9/25` | `DATE: [MM/DD/YY]` |
| COMPANY | `COMPANY:` | `COMPANY: [Name]` |
| HQ | `HQ: ` | `HQ: [City, State]` |
| STAGE | `STAGE: ` | `STAGE: [Pre-Seed/Seed/Series A]` |
| INVESTMENT SOUGHT | `INVESTMENT SOUGHT: ` | `INVESTMENT SOUGHT: [$XM]` |
| REACH CONTACT | `REACH CONTACT: ` | `REACH CONTACT: [Initials]` |
| TEAM | `TEAM: ` | `TEAM: [Founder names]` |

For content sections, find the section header followed by an empty `<w:r>` element and insert text:

```python
# Pattern for filling empty sections
old = '''<w:t xml:space="preserve">PRODUCT DESCRIPTION</w:t>
            </w:r>
          </w:p>
          <w:p ...>
            ...
            <w:r ...>
              <w:rPr>
                <w:rtl w:val="0"/>
              </w:rPr>
            </w:r>
          </w:p>'''

new = '''<w:t xml:space="preserve">PRODUCT DESCRIPTION</w:t>
            </w:r>
          </w:p>
          <w:p ...>
            ...
            <w:r ...>
              <w:rPr>
                <w:rFonts w:ascii="Proxima Nova" .../>
                <w:rtl w:val="0"/>
              </w:rPr>
              <w:t xml:space="preserve">[Your content here]</w:t>
            </w:r>
          </w:p>'''
```

### Sections to Fill

**Header (left column):**
- DATE, COMPANY, HQ, STAGE, INVESTMENT SOUGHT, REACH CONTACT, TEAM

**Overview (3-column box):**
- PRODUCT DESCRIPTION (1-2 sentences)
- IMPACT THESIS (1 sentence)
- UNFAIR ADVANTAGE (1 sentence)

**PROS section (with subcategories):**
- PRODUCT - bullet point
- MARKET - bullet point
- TRACTION - bullet point
- TEAM - bullet point (include founder backgrounds from research)
- FUNDRAISING - Current round: / Past rounds: (from research)

**CONS section:**
- RISKS - bullet point(s)

**Bottom row (2 columns):**
- QUESTIONS FOR REACH TEAM (include gaps from research)
- RECOMMENDATIONS

---

## Content Guidelines

### Integrating Research into Content

**Team section should include researched backgrounds:**
> **Strong and relevant team**
> - [Founder]: [Prior company] where they [relevant accomplishment]. [Education if notable].
> - [Founder]: Previously [role] at [company]. [Domain expertise].

**Fundraising should include history:**
> **Fundraising**
> - Current round: $3M Seed
> - Past rounds: $500K pre-seed from [Investors] (2023)

**Questions should capture research gaps:**
> - Unclear on [Founder]'s specific role at [Prior Company] - was it a successful exit?
> - Could not verify claimed [metric] - worth confirming in diligence

### Inferring Impact Thesis

If not explicitly stated, infer by asking:
- What social problem does this solve?
- Who benefits and how?
- What's the broader positive outcome?

**Examples:**
- Fintech for underbanked → "Expands financial access to underserved populations"
- AI for healthcare → "Improves patient outcomes through earlier/better diagnosis"
- EdTech → "Democratizes access to quality education"

### Inferring Unfair Advantage

Look for (including from research):
- Founder background (domain expertise, prior exits, unique access)
- Proprietary data or technology
- Network effects or switching costs
- First-mover advantage in a specific vertical

**Examples:**
- "Founder spent 5 years at [Industry Leader], built the core [product]"
- "Team includes ex-[Competitor] engineers who know the technical gaps"
- "Prior exit to [Acquirer] for $[X]M in same space"

### Writing Guidelines

**Voice:**
- Direct, analytical, no fluff
- Use specific numbers over vague qualifiers
- Lead bullets with the strongest point
- Include specifics from research (names, companies, numbers)

**Length discipline:**
- Product description: 1-2 sentences
- Impact thesis: 1 sentence
- Unfair advantage: 1 sentence
- Each pro/con bullet: 1-2 lines max
- Total: Must fit on 1 page (landscape)

### Good Examples (with research-enriched content)

**Good Pro (Team) - Research Enriched:**
> **Very strong and relevant team**
> - Luis: MIT BS/MS in computer science, co-founded Immunai (AI cancer immunotherapy, now valued at $1BN)
> - Bill: Chief Clinical Officer; professor of psychology at Dartmouth. CEO of 4 mental health companies.

**Good Pro (Traction + Context):**
> $270k ARR, 5x growth since Q1. Notable customers include Carrot Fertility and Ryder Logistics.

**Good Con (with research context):**
> - Round dynamics: Series A bet at seed-level traction. Raised $8M seed already, valuation expectations will be high.

**Bad (missing easily researched context):**
> Team is experienced (no specifics on where/what)
> Previously raised money (no amounts or investors)

---

## Output

Save the final document to `/mnt/user-data/outputs/[CompanyName]_First_Look.docx`

---

## Quick Reference: Research Checklist

Before writing, confirm you have (or searched for):

- [ ] Company HQ location
- [ ] Current funding stage and amount sought
- [ ] Prior funding rounds (amounts, investors)
- [ ] Founder full names and titles
- [ ] Founder backgrounds (prior companies, roles, exits)
- [ ] Product description (if deck unclear, check website)
- [ ] Any notable investors or advisors

If any critical item remains unknown after 2-3 min research, add to QUESTIONS section.

---

## Template Reference

See `References/First_Look_Template.docx` for the actual template file.
See `References/examples.md` for content examples from completed First Looks.