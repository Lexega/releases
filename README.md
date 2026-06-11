# Lexega SQL Releases

Pre-built binaries for [Lexega](https://lexega.com) — pre-execution risk analysis for SQL. Supported dialects: Snowflake, PostgreSQL, BigQuery, MSSQL, Databricks, and Redshift.

## Install (recommended)

```bash
curl -sSL https://lexega.com/install.sh | sh
```

Detects your platform, downloads the latest release, verifies the SHA-256 checksum, and installs to `~/.local/bin`.

## Manual Download

Download from the [Releases page](https://github.com/Lexega/releases/releases/latest). Every release includes a `CHECKSUMS.sha256` file and a CycloneDX SBOM per binary.

| Platform | Binary |
|----------|--------|
| Linux x64 | `lexega-sql-linux-x64` |
| Linux ARM64 | `lexega-sql-linux-arm64` |
| macOS Intel | `lexega-sql-darwin-x64` |
| macOS Apple Silicon | `lexega-sql-darwin-arm64` |
| Windows x64 | `lexega-sql-windows-x64.exe` |
| Windows ARM64 | `lexega-sql-windows-arm64.exe` |

```bash
# Linux / macOS — pinned, checksum-verified
VERSION=v1.11.0                # pick from the Releases page
ASSET=lexega-sql-linux-x64     # linux/darwin × x64/arm64
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
	curl -sSL -O https://github.com/Lexega/releases/releases/download/v1.11.0/lexega-sql-linux-x64
	curl -sSL https://github.com/Lexega/releases/releases/download/v1.11.0/CHECKSUMS.sha256 | grep ' lexega-sql-linux-x64$' | sha256sum -c -
	sudo install -m 755 lexega-sql-linux-x64 /usr/local/bin/lexega-sql

- name: SQL Review
  env:
	LEXEGA_LICENSE_KEY: ${{ secrets.LEXEGA_LICENSE_KEY }}
  run: lexega-sql review ${{ github.event.pull_request.base.sha }}..${{ github.sha }} . -r
```

Full pipeline examples (GitLab, Bitbucket, Azure DevOps, PR comments, SARIF upload): [CI/CD Integration docs](https://lexega.com/docs/integration).

## License Key

Formatting is free forever — no key needed. Risk analysis requires a license key: [get a free trial key](https://lexega.com/#get-early-access)(instant, self-serve), then set `LEXEGA_LICENSE_KEY`.

## Links

- [Quick Start](https://lexega.com/docs/quick-start)
- [Documentation](https://lexega.com/docs)
- [Website](https://lexega.com)
