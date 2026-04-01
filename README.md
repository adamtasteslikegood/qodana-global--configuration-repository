# Qodana Global Configuration Repository

Centralized [Qodana](https://www.jetbrains.com/qodana/) inspection profiles for the **adamtasteslikegood** organization. Changes pushed to this repo are automatically uploaded to Qodana Cloud, where they apply to every linked project.

## Repository structure

```
├── qodana-global-configurations.yaml   # Catalog — maps profile IDs → YAML files
├── base/qodana.yaml                    # Shared baseline (security, licensing)
├── profiles/
│   ├── python/qodana.yaml              # Python / Django / Jinja2
│   ├── js-ts/qodana.yaml              # JavaScript / TypeScript
│   ├── jvm/qodana.yaml                # Java / Kotlin (Spring, Lombok)
│   ├── web/qodana.yaml                # HTML / CSS / Accessibility
│   ├── go/qodana.yaml                 # Go
│   ├── dotnet/qodana.yaml             # .NET (C#/F#/VB)
│   ├── php/qodana.yaml                # PHP
│   └── fullstack/qodana.yaml          # Multi-language projects
├── teamA/qodana.yaml                  # Legacy team A profile
├── teamB/qodana.yaml                  # Legacy team B profile
└── .github/
    ├── dependabot.yml                  # Keeps Actions & Docker images up to date
    └── workflows/
        ├── upload-global-configuration.yml  # Validates YAML then uploads to Qodana Cloud
        ├── dependency-review.yml            # PR dependency audit
        └── daily-repo-status.md             # Daily report agent (Copilot)
```

## Profiles overview

| Profile ID   | Linter                         | Languages / Frameworks                  | Coverage gates       |
|--------------|--------------------------------|----------------------------------------|---------------------|
| `base`       | *(any)*                        | Cross-language security & licensing     | —                   |
| `python`     | `jetbrains/qodana-python`      | Python, Django, Jinja2                  | 40% total / 50% new |
| `js_ts`      | `jetbrains/qodana-js`          | JavaScript, TypeScript, HTML            | 40% total / 50% new |
| `jvm`        | `jetbrains/qodana-jvm`         | Java, Kotlin, Spring, Lombok            | 50% total / 60% new |
| `web`        | `jetbrains/qodana-js`          | HTML, CSS, Accessibility                | —                   |
| `go`         | `jetbrains/qodana-go`          | Go                                      | 50% total / 60% new |
| `dotnet`     | `jetbrains/qodana-dotnet`      | C#, F#, VB.NET                          | 50% total / 60% new |
| `php`        | `jetbrains/qodana-php`         | PHP                                     | —                   |
| `fullstack`  | *(per-language)*               | Multi-language projects                 | 40% total / 50% new |

Every profile inherits `base/qodana.yaml`, which enforces:
- **Zero critical findings** — builds fail on any critical issue
- **Security inspections** — enabled across all supported languages
- **License compliance** — `CheckDependencyLicenses` enabled
- **Hardcoded passwords** — detected and flagged

## CI/CD workflows

### Upload workflow (`upload-global-configuration.yml`)

Runs on pushes to **main**, **master**, **dev**, **develop**, `dev/**`, and `development/**` branches plus PRs and manual dispatch.

1. **Validates** all YAML files with yamllint
2. **Uploads** the configuration catalog to Qodana Cloud

Requires the `QODANA_CONFIGURATIONS_TOKEN` repository/organization secret.

### Dependency review (`dependency-review.yml`)

Audits dependency changes on every PR against main/dev branches. Fails on high/critical vulnerabilities and blocks copyleft-incompatible licenses.

### Daily status report (`daily-repo-status.md`)

A Copilot agent workflow that creates a daily GitHub issue summarizing repository activity, workflow health, and actionable next steps.

### Dependabot (`dependabot.yml`)

Keeps GitHub Actions and Docker image references up to date with weekly checks.

## Using a profile in your project

In your project's `qodana.yaml`:

```yaml
version: "1.0"
linter: jetbrains/qodana-python:2025.1
```

Then assign the corresponding global profile (e.g., `python`) to that project in Qodana Cloud.

## Adding a Qodana workflow to a new repo

Add this workflow to `.github/workflows/qodana.yml` in the target repo:

```yaml
name: Qodana
on:
  workflow_dispatch:
  pull_request:
  push:
    branches:
      - main
      - master
      - 'dev/**'
      - 'development/**'

jobs:
  qodana:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write
      checks: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Qodana Scan
        uses: JetBrains/qodana-action@v2025.1
        env:
          QODANA_TOKEN: ${{ secrets.QODANA_TOKEN }}
```

## Requirements

- A **JetBrains All Access** subscription (provides access to all Qodana linters)
- `QODANA_CONFIGURATIONS_TOKEN` secret configured at the organization or repository level
- `QODANA_TOKEN` secrets configured in each individual repository for Qodana scans

## Useful links

- [Qodana Global Configuration docs](https://www.jetbrains.com/help/qodana/global-configuration.html)
- [Qodana GitHub Action](https://github.com/JetBrains/qodana-action)
- [Qodana linter list](https://www.jetbrains.com/help/qodana/linters.html)
- [Qodana Cloud](https://qodana.cloud)
