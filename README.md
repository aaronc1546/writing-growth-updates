# Writing Growth Updates

Claude Code skill for writing weekly JIRA project updates for Self-Serve Growth team meetings.

## Install

```bash
claude skill add --from https://github.com/aaronc1546/writing-growth-updates
```

## Usage

In Claude Code, run:

```
/writing-growth-updates
```

The skill walks you through gathering context, drafting JIRA updates, Coda entries, and Slack messages for each project.

## Setup

The skill expects a `Plans/CURRENT.md` file with a `jira_projects` list:

```yaml
jira_projects:
  - id: EPDP-3307
    name: "Project name"
    solution_path: Path/To/Feature
    slack_channel: C04PLL934F4
```

## What's Included

- `SKILL.md` - Main skill definition
- `references/` - Real examples for format reference
  - `jira-update-ltv2-nonsearch-w03.md` - JIRA update example
  - `coda-entry-examples.md` - Coda doc entry examples
  - `slack-message-example.md` - Slack message format
