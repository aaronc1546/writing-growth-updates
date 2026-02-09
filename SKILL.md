---
name: writing-growth-updates
description: Writes weekly JIRA project updates for Self-Serve Growth team meetings. Use when preparing growth updates, writing JIRA updates, preparing for self-serve growth weekly, or helping with the Monday growth meeting.
---

# Writing Growth Updates

## Key Principle: Fresh Context Per Project

Each JIRA project update is completed in a **separate conversation**. After completing one project:
1. User copies final draft to JIRA
2. Optional: write Coda doc entry
3. User clears context
4. User invokes skill again for next project

This prevents cross-contamination between unrelated projects.

---

## Inputs

| File | Purpose |
|------|---------|
| `Plans/CURRENT.md` | Get `jira_projects` list (includes `slack_channel` per project) |
| `<Solution>/Knowledge/Synthesis/SYNTHESIS.md` | Pre-analyzed meeting insights (if exists) |
| `<Solution>/Knowledge/` | Context from past 7 days |
| `<Solution>/IDEAS.md` | Active ideas for the solution |
| `<Solution>/Weekly Updates/YYYY-WW/` | Check if already drafted this week |
| Slack channel (per project) | Recent messages from project's configured channel |
| `references/` | Reference examples (DO NOT ask user for templates) |

---

## Configuration

Each project in `jira_projects` (in `Plans/CURRENT.md`) can specify a `slack_channel`:

```yaml
jira_projects:
  - id: EPDP-3307
    name: "Enable LTV 2.0 value-based bidding on search"
    solution_path: Superhuman/Problems/...
    slack_channel: C04PLL934F4  # team-ad-bidding-optimization
```

**If no slack_channel is configured:** Prompt the user: "I don't have a Slack channel configured for this project. What's the channel ID? (You can skip if there isn't one.)" Then update `Plans/CURRENT.md` with the provided channel ID for future sessions.

---

## Examples

**IMPORTANT:** Read the assets folder BEFORE drafting. These are real examples created through this workflow and represent the exact format to follow.

- `references/jira-update-ltv2-nonsearch-w03.md` — Canonical JIRA update example (use this format exactly)
- `references/coda-entry-examples.md` — Coda doc entry examples
- `references/slack-message-example.md` — Slack message format for team channel

Follow these examples precisely for structure, tone, and level of detail.

---

## Process

### Phase 1: Status Check

1. Read `Plans/CURRENT.md` to get `jira_projects` list
2. For each project, check `<feature_path>/Weekly Updates/YYYY-WW/` for existing drafts
3. Present status:

```
## Growth Update Status (W03)

| Project | JIRA | Status |
|---------|------|--------|
| LTV 2.0 on search | EPDP-3307 | Done |
| Gen AI Creative | EPDP-2833 | Not started |
| LTV 2.0 non-search | EPDP-2513 | Not started |

Suggested next: Gen AI Creative (EPDP-2833)
```

4. Confirm which project to work on

5. **Ask update type:**

```
What type of update is this?
1. **Quick update** — No significant changes, reuse last week's structure
2. **Substantive update** — New developments requiring full context gathering
```

**If Quick update:** Skip Phase 2, go directly to Phase 3 Step C. User provides the key points to include (or "same as last week").

---

### Phase 2: Context Review

For the selected project only:

1. Read feature's `CLAUDE.md` for context
2. **Check for synthesis file** at `<Feature>/Knowledge/Synthesis/SYNTHESIS.md`
   - If exists, read the most recent synthesis entry (top section)
   - Note when it was created and what meetings it covers
3. Scan `Knowledge/` folder for recent additions (past 7 days)
4. **Read ALL Granola transcripts from the past 7 days** — cross-reference multiple meetings to build a complete picture before drafting. Don't just read the most recent one.
5. Read `IDEAS.md` for active ideas being considered
6. **Search Slack for recent context** (if Slack MCP available - READ ONLY, never post)
   - First check if project has a `slack_channel` configured in `Plans/CURRENT.md`
   - If no channel configured, prompt user for channel ID (or skip if none exists)
   - Pull channel history (limit: 20 messages minimum)
   - **For each message with a `thread_ts`, fetch the full thread** using `slack_get_thread_replies` — most substantive discussion happens in threads; parent messages alone miss critical context
   - Extract key themes: decisions made, blockers raised, progress updates, questions asked
   - Note any action items or commitments mentioned
   - If Slack MCP not connected, note in context summary and proceed without
7. Summarize what's available:

```
## Available Context for [Project Name]

**Meeting Synthesis:**
- Last synthesis: YYYY-MM-DD (covers X meetings)
- Key insights: [brief summary of patterns/decisions]
- OR: No synthesis file found

**From Knowledge folder:**
- [file] — [brief description]

**From IDEAS.md:**
- Active ideas: [X ideas]
- Recently captured: [Y ideas from past 7 days]

**From Slack (#channel-name):**
- [X messages found in past 7 days]
- Key themes: [decisions, blockers, progress]
- Notable updates: [specific items worth highlighting]
- OR: No relevant messages found
- OR: Slack MCP not connected — skipped

**Gaps:**
- No meeting notes from this week
- No performance data
```

**Why synthesis files matter:** The `/synthesize-project-meetings` skill creates deep analysis of meeting transcripts. If a recent synthesis exists, use it as a primary context source rather than re-reading raw transcripts. This saves time and provides pre-analyzed patterns.

---

### Phase 2.5: Context Interrogation

After gathering context, probe the user to ensure completeness and push on implications. Use AskUserQuestion for interactive dialogue.

**Round 1: Significance (always ask)**
- "Based on what I see, [X] and [Y] seem like the main developments. Is that right, or is there something more significant I'm missing?"
- "What was the most important thing that happened this week?"

**Round 2: Implications (if pivots, blockers, or decisions)**
- For pivots/decisions: "You decided [X]. What does that mean for [related work]? Timeline implications?"
- For blockers: "You're waiting on [X]. What happens if that doesn't come through?"
- For progress: "This moved forward. Does that unlock anything else or change priorities?"

**Round 3: What's Next (if strategic inflection or uncertainty)**
- "Are these the right priorities for next week, or is there something you're not surfacing?"
- "What's the riskiest assumption in your plan?"
- "What would make this project fail in the next 2 weeks?"

**Depth heuristic:**
- Always: Round 1
- If pivots/blockers/decisions detected in context: Add Round 2
- If strategic inflection point or user seems uncertain: Add Round 3

Summarize what you learned from interrogation before proceeding to drafting.

---

### Phase 3: JIRA Update

**Step A: Context Synthesis**

**Infer "What Next" from transcripts** — don't ask the user what's next if meeting notes cover it. Propose based on what you see and ask for confirmation.

**Check for paused/pivoted tracks** — if any workstream was paused, pivoted, or had a major decision this week, include full context:
- What happened (the decision)
- Why (the reasoning)
- What's next for that track
- Forward-looking stance if applicable (e.g., "We still believe X is the right long-term path and will revisit when Y")

Present proposed storylines:

```
Based on system context, here's what I see:

**Proposed Highlights:**
1. [What happened] — [Why it matters]
2. [What happened] — [Why it matters]

**Proposed What Next:**
1. [Action] — [Context]

**Questions to fill gaps:**
```

Write questions to: `<Feature>/Weekly Updates/YYYY-WW/questions.md`

Ask user to review questions file and provide answers inline.

---

**Step B: User Input**

User can provide:
- Screenshots (Slack, metrics, dashboards)
- Granola meeting transcripts
- Brain dump / stream of consciousness

Save additional context to: `<Feature>/Weekly Updates/YYYY-WW/context/`

**Granola Transcripts:**

When user provides Granola meeting transcripts:
1. Save each to `<Feature>/Knowledge/Meeting Transcripts/YYYY-MM-DD-<Meeting-Title>.md`
2. Format with: Meeting title as H1, Date, Participants, then transcript
3. Use these for context synthesis alongside other Knowledge files

---

**Step C: Draft Update**

Create: `<Feature>/Weekly Updates/YYYY-WW/draft-v1.md`

Use this format for the **draft file** (includes title for local reference):

```markdown
# [Project Name] — Weekly Update W03

## TL;DR
[2-3 sentences MAX: 1-2 sentences on most critical highlight(s), 1 sentence on what's next]

## Highlights
- **[What happened]**: [Why it matters]
- **[What happened]**: [Why it matters]

## What Next
- **[Action]** [context/rationale]
- **[Action]** [context]
```

**JIRA format** (when posting to JIRA, omit the title — start directly with TL;DR):

```markdown
**TL;DR**
[content]

**Highlights**
- **[What happened]**: [Why it matters]

**What Next**
- **[Action]** [context]
```

**Format guidelines:**
- TL;DR: Focus on broadly interesting/critical items only. NOT a comprehensive summary.
- Highlights: Curate to significant items. Each bullet = what happened + why it matters.
- What Next: Bold action with context.
- Punctuation: Use regular hyphens, not em-dashes
- Audience: working team (primary), managers, growth leadership (accessible to all)

---

**Step D: Iterate**

Present draft to user. On feedback:
- Create `draft-v2.md`, `draft-v3.md`, etc.
- Never overwrite previous versions
- Continue until user says "done"

---

**Step E: Finalize JIRA**

When user approves the draft:
1. **Post directly to JIRA** using `mcp__plugin_atlassian_atlassian__addCommentToJiraIssue`
   - cloudId: `232911b9-e00e-4f97-8caf-2e4a5b267020` (Grammarly)
   - issueIdOrKey: project's JIRA ID (e.g., EPDP-3307)
   - commentBody: the update in JIRA format (no title header)
2. **Provide link** to the posted comment: `https://grammarly.atlassian.net/browse/{JIRA-ID}?focusedCommentId={commentId}`

If Atlassian MCP is not connected, fall back to asking user to copy manually.

---

### Phase 4: Coda Doc Entry

After JIRA is updated:

> "Does this project warrant a Coda doc entry this week?"
>
> Entry types: Highlights, Lowlights, Requests for help, Decisions needed, Launches, FYIs

**If yes:** Draft entry in `Superhuman/Weekly Updates/YYYY-WW/coda-entries.md`

**Always offer 2-3 versions** with different angles/tones for the user to choose from. Coda entries are high-visibility and framing matters.

**Coda entry guidelines:**
- **Headline = broadly interesting news, not metrics** — lead with the decision or news, put specific numbers in the body
- **When pivoting away from an approach**, include forward-looking stance if applicable (e.g., "We still believe X is the most scalable path long-term and will work in parallel to improve Y")
- Keep it scannable — these are read quickly in a table

**Coda entry format:**
```
**[Bold headline - the news].** [1-2 sentences of context/explanation]

What's next: [1-2 sentences on next steps]
```

Append to file if it already exists.

---

### Phase 5: Slack Message Draft

After JIRA and Coda decisions, draft a Slack message for the team channel.

**NEVER send the message** — write to file only for user to copy/paste.

Create: `<Feature>/Weekly Updates/YYYY-WW/slack-message.md`

**Format:**
```markdown
# Slack Message Draft

Copy and paste into #team-ad-bidding-optimization:

---

:memo: <https://grammarly.atlassian.net/browse/{JIRA-ID}|Weekly update>
:{status_emoji}: **{Status}.** {TL;DR from the update}

{One paragraph with key details: who's doing what this week, notable timeline/blocker info, what to watch for}
```

**Status options:**
- `:large_green_circle:` **On track.** — proceeding as planned, no blockers
- `:large_yellow_circle:` **At risk.** — potential blockers or timeline concerns
- `:red_circle:` **Off track.** — blocked or behind schedule

**Ask user:** "Is this project on track, at risk, or off track?"

Then generate the message: TL;DR line + one concise paragraph with the most important context (who's doing what, key dates, blockers). Not too short, not too long.

---

### Phase 6: Done

Confirm completion:

```
## [Project Name] Complete

- JIRA updated: Done
- Coda entry: [Written / Skipped]
- Slack message: Drafted (copy from slack-message.md)
- Draft location: <Feature>/Weekly Updates/YYYY-WW/

**Next:** Clear context and invoke skill again for remaining projects:
- [ ] [Project 2]
- [ ] [Project 3]

**Handoff prompt for next project:**
[Generate a minimal prompt the user can paste into a fresh session]
```

**Handoff prompt format:**
```
Run /writing-growth-updates for [Project Name] (JIRA: [ID]).

Week: W[XX]
Update type: [Quick/Substantive]
Key context: [1-2 sentences on what happened this week, or "No change from last week"]
```

This allows starting a fresh session with minimal context loading.

---

## Universal Style Guide

Also read `Knowledge/WRITING-STYLE.md` for universal writing style rules that apply to all communications.

## Validation Checklist

Before finalizing each update:
- [ ] Context interrogation completed (Round 1 minimum; Rounds 2-3 if applicable)
- [ ] ALL Granola transcripts from past 7 days read and cross-referenced (if substantive update)
- [ ] Slack channel searched: 20+ parent messages AND all threads fetched (if Slack MCP available)
- [ ] Any paused/pivoted tracks have full context (what, why, what's next, forward-looking stance)
- [ ] "What Next" inferred from transcripts, not just asked
- [ ] TL;DR is 2-3 sentences max (not dense paragraphs)
- [ ] Highlights focus on significance, not just activity
- [ ] What Next has bold actions with context
- [ ] Accessible to leadership (no unexplained jargon)
- [ ] Draft versioned (v1, v2, etc.) — never overwrite
- [ ] Coda entry: 2-3 versions offered if applicable
- [ ] Slack message drafted with status indicator (on track / at risk / off track)

---

## Do NOT

- **NEVER post, react, or reply to Slack** — this workflow is READ ONLY for Slack
- **Ask user for templates or example formats** — read `references/` instead
- Use em-dashes in drafts - restructure sentences or use colons/commas/parentheses instead (em-dashes are an AI writing tell)
- Mix context from multiple projects in one conversation
- Write dense 4+ sentence TL;DRs (keep it short)
- Include activity without explaining significance
- Create drafts without user confirmation
- Overwrite previous draft versions
- Proceed to next project without clearing context
