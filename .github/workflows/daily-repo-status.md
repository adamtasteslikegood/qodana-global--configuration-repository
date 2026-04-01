---
description: |
  Daily status report for the Qodana Global Configuration repository.
  Gathers recent repository activity (issues, PRs, configuration changes)
  and generates a GitHub issue with an actionable summary.

on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

network: defaults

tools:
  github:
    lockdown: false

safe-outputs:
  mentions: false
  allowed-github-references: []
  create-issue:
    title-prefix: "[daily-status] "
    labels: [report, daily-status]

source: githubnext/agentics/workflows/daily-repo-status.md@442992eda2ccb11ee75a39c019ec6d38ae5a84a2
engine: copilot
---

# Daily Repo Status

Create an upbeat daily status report for this Qodana Global Configuration repository as a GitHub issue.

## What to include

- Recent configuration changes (new profiles, updated inspections, threshold changes)
- Open issues and pull requests — progress & blockers
- Configuration upload workflow status (passing / failing)
- Any new language profiles added or modified
- Recommendations for profile improvements or missing coverage

## Style

- Be positive, encouraging, and helpful 🌟
- Use emojis moderately for engagement
- Keep it concise — adjust length based on actual activity

## Process

1. Gather recent activity from the repository (last 24 hours)
2. Check the status of recent workflow runs
3. Review open issues and pull requests
4. Create a new GitHub issue with findings and actionable next steps
