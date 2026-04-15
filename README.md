# <span data-proof="authored" data-by="ai:claude">VC Skills</span>

<span data-proof="authored" data-by="ai:claude">Claude Skills for VC workflow: early-stage deal evaluation, from first-look triage through full investment committee memo.</span>

## <span data-proof="authored" data-by="ai:claude">The 3-Step Memo Pipeline</span>

<span data-proof="authored" data-by="ai:claude">The core workflow is a three-step pipeline. Each step is its own skill, and the orchestrator runs them in sequence.</span>

1. **[<span data-proof="authored" data-by="ai:claude">evidence-ledger</span>](./evidence-ledger)** <span data-proof="authored" data-by="ai:claude">— Extract and classify every fact from deal inputs (pitch deck, call notes, data room, references) into a tagged, structured Evidence Ledger.</span>
2. **[<span data-proof="authored" data-by="ai:claude">context-pack</span>](./context-pack)** <span data-proof="authored" data-by="ai:claude">— Verify claims, fill gaps, and surface market signals through hypothesis-driven research. Takes the Evidence Ledger as input.</span>
3. **[<span data-proof="authored" data-by="ai:claude">investment-memo</span>](./investment-memo)** <span data-proof="authored" data-by="ai:claude">— Write the final IC memo. Requires the completed Evidence Ledger + Context Pack.</span>

**[<span data-proof="authored" data-by="ai:claude">full-investment-memo-orchestrator</span>](./full-investment-memo-orchestrator)** <span data-proof="authored" data-by="ai:claude">— Orchestrator that shows which skills to run in which order. Use when you have a full data room, multiple calls, and references.</span>

## <span data-proof="authored" data-by="ai:claude">Lighter-weight Memos</span>

* **[<span data-proof="authored" data-by="ai:claude">first-look-generator</span>](./first-look-generator)** <span data-proof="authored" data-by="ai:claude">— 1-page summary from raw inputs (pitch deck, call notes). Used for deal pipeline triage before committing to the full pipeline.</span>

* **[<span data-proof="authored" data-by="ai:claude">pre-IC-memo</span>](./pre-IC-memo)** <span data-proof="authored" data-by="ai:claude">— 4-page Pre-IC memo from a pitch deck and/or call notes. Includes Gokul Rajaram's 8 Moats defensibility scoring. Relies on external research to fill gaps.</span>

## <span data-proof="authored" data-by="ai:claude">Installation</span>

<span data-proof="authored" data-by="ai:claude">Drop any skill folder into</span> <span data-proof="authored" data-by="ai:claude">`~/.claude/skills/`</span> <span data-proof="authored" data-by="ai:claude">(or your Cowork / Claude Skills workspace) and Claude will pick it up.</span>