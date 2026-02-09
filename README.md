# Writing Growth Updates

Claude Code skill for writing weekly JIRA project updates for Self-Serve Growth team meetings.

## What It Does

Walks you through a structured workflow for each project:

1. **Gathers context** from Slack, Granola meeting transcripts, and Glean
2. **Asks you probing questions** to surface what actually matters this week
3. **Drafts a JIRA update** (TL;DR + Highlights + What Next)
4. **Optionally drafts** a Coda doc entry and Slack message

Each project gets its own conversation to avoid cross-contamination.

## Install

```bash
claude skill add --from https://github.com/aaronc1546/writing-growth-updates
```

## Prerequisites

### 1. Claude Code

You need [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working.

### 2. MCP Servers (for context gathering)

The skill pulls context from these sources. The more you have, the better the drafts:

| MCP Server | Purpose | Required? |
|------------|---------|-----------|
| **Slack** or **Glean** | Reads recent channel messages and threads for project context | Recommended (pick one) |
| **Granola** | Reads meeting transcripts from the past 7 days ([sync-granola](https://github.com/aaronc1546/sync-granola) skill recommended) | Recommended |
| **Atlassian** | Posts finished updates directly to JIRA | Optional (can copy/paste instead) |

Without any MCP servers, the skill still works. You'll just provide context manually (brain dump, screenshots, etc.) and copy the final draft to JIRA yourself.

### 3. File Structure

The skill expects a specific folder layout. Create this in your Claude Code project root:

```
your-project/
├── Plans/
│   └── CURRENT.md          # Project configuration (see below)
└── <your-project-folder>/   # One folder per JIRA project
    ├── Knowledge/           # Drop meeting notes, context docs here
    │   └── Granola/         # Meeting transcripts (if using Granola)
    └── Weekly Updates/      # Skill writes drafts here
        └── YYYY-WW/         # e.g., 2026-07/
            ├── draft-v1.md
            └── slack-message.md
```

You only need to create `Plans/CURRENT.md` and one folder per project. The skill creates `Weekly Updates/` subfolders automatically.

### 4. Configure `Plans/CURRENT.md`

This is how the skill knows which projects you own. Create `Plans/CURRENT.md` with:

```yaml
jira_projects:
  - id: EPDP-3307
    name: "Enable LTV 2.0 value-based bidding on search"
    solution_path: Projects/LTV-2.0-Search
    slack_channel: C04PLL934F4  # Optional - skill will ask if missing
  - id: EPDP-2833
    name: "Gen AI Creative Loop"
    solution_path: Projects/Gen-AI-Creative
    slack_channel: C05ABC1234
```

**Fields:**
- `id`: Your JIRA epic/project ID
- `name`: Human-readable project name
- `solution_path`: Path to the project folder (relative to your project root)
- `slack_channel`: Slack channel ID for context gathering (optional; the skill will prompt you if missing)

## Usage

```
/writing-growth-updates
```

The skill will:
1. Show you which projects need updates this week
2. Ask which one to work on
3. Ask if it's a **quick update** (no changes, reuse last week's structure) or **substantive update** (full context gathering)
4. Gather context and draft the update
5. Iterate with you until you're happy
6. Post to JIRA (or give you the text to copy)

## What to Customize

The skill and reference examples reflect how our team currently writes updates. Things you might want to adjust:

- **`references/`** — These are real examples showing the exact format. If your project's updates look different, replace these with your own examples.
- **Atlassian cloudId** — The SKILL.md has our Grammarly Atlassian cloudId hardcoded (`232911b9-e00e-4f97-8caf-2e4a5b267020`). If you're posting to a different Atlassian instance, update this in SKILL.md.
- **Coda entries** — Phase 4 drafts entries for the Self-Serve Growth Coda doc. Skip this phase if your team doesn't use it.

## Tips

- Run the skill once per project, then clear context before starting the next one
- The "quick update" option is great for weeks where nothing changed
- Drop screenshots, Slack threads, or brain dumps when the skill asks for additional context
- The skill never overwrites previous draft versions (v1, v2, v3...)
