# Lexega Releases

Pre-built binaries for [Lexega](https://lexega.com): pre-execution analysis for SQL. Dialects: Snowflake, T-SQL (SQL Server), BigQuery, PostgreSQL, Redshift, Databricks and MySQL.

## What a release contains

| File | What it is |
|------|------------|
| `lexega-sql-<platform>` | The full command line. Formatting, analysis and review run without a key; a license key adds semantic analysis, dbt/Jinja rendering, semantic diff, policy enforcement, the dashboard and cross-script analysis. |
| `lexega-<platform>` | The open edition, built from [Lexega/lexega](https://github.com/Lexega/lexega) (source-available, BUSL-1.1). |
| `lexega-sf-catalog-<platform>`, `lexega-dbx-catalog-<platform>`, `lexega-mssql-catalog-<platform>` | Catalog extractors for Snowflake, Databricks and SQL Server, run by `catalog pull`. |
| `CHECKSUMS.sha256` | SHA-256 of every file in the release. |
| `sbom-<name>.cdx.json` | A CycloneDX SBOM for each binary and each extractor. |

## Install (recommended)

```bash
curl -sSL https://lexega.com/install.sh | sh
```

Detects your platform, downloads the latest `lexega-sql`, verifies its SHA-256 checksum, and installs to `~/.local/bin`.

## Manual download

Download from the [Releases page](https://github.com/Lexega/releases/releases/latest).

| Platform | Full command line | Open edition |
|----------|-------------------|--------------|
| Linux x64 | `lexega-sql-linux-x64` | `lexega-linux-x64` |
| Linux ARM64 | `lexega-sql-linux-arm64` | `lexega-linux-arm64` |
| macOS Intel | `lexega-sql-darwin-x64` | `lexega-darwin-x64` |
| macOS Apple Silicon | `lexega-sql-darwin-arm64` | `lexega-darwin-arm64` |
| Windows x64 | `lexega-sql-windows-x64.exe` | `lexega-windows-x64.exe` |
| Windows ARM64 | `lexega-sql-windows-arm64.exe` | `lexega-windows-arm64.exe` |

```bash
# Linux / macOS, pinned and checksum-verified
VERSION=v2.0.0                 # pick from the Releases page
ASSET=lexega-sql-linux-x64     # linux/darwin x x64/arm64
curl -sSL -O "https://github.com/Lexega/releases/releases/download/${VERSION}/${ASSET}"
curl -sSL "https://github.com/Lexega/releases/releases/download/${VERSION}/CHECKSUMS.sha256" | grep " ${ASSET}$" | sha256sum -c -
sudo install -m 755 "${ASSET}" /usr/local/bin/lexega-sql

# Verify
lexega-sql --version
```

## CI/CD

```yaml
# GitHub Actions
steps:
  - uses: actions/checkout@v4
    with:
      fetch-depth: 0   # review compares against the PR base

  - name: Install Lexega
    run: |
      curl -sSL -O https://github.com/Lexega/releases/releases/download/v2.0.0/lexega-sql-linux-x64
      curl -sSL https://github.com/Lexega/releases/releases/download/v2.0.0/CHECKSUMS.sha256 | grep ' lexega-sql-linux-x64$' | sha256sum -c -
      sudo install -m 755 lexega-sql-linux-x64 /usr/local/bin/lexega-sql

  - name: SQL Review
    env:
      LEXEGA_LICENSE_KEY: ${{ secrets.LEXEGA_LICENSE_KEY }}   # optional: adds semantic analysis and policy enforcement
    run: lexega-sql review ${{ github.event.pull_request.base.sha }}..${{ github.sha }} . -r
```

Full pipeline examples (GitLab, Bitbucket, Azure DevOps, PR comments, SARIF upload): [CI/CD Integration docs](https://lexega.com/docs/integration).

## License key

Formatting, analysis and review run without a key. A license key adds semantic analysis, dbt/Jinja rendering, semantic diff, policy enforcement, the dashboard and cross-script analysis. [Get a free 30-day trial key](https://lexega.com/#get-started) or [buy Team](https://lexega.com/buy), then set `LEXEGA_LICENSE_KEY` or run `lexega-sql license activate`.

## Links

- [Quick Start](https://lexega.com/docs/quick-start)
- [Documentation](https://lexega.com/docs)
- [Source of the open edition](https://github.com/Lexega/lexega)
- [Website](https://lexega.com)
