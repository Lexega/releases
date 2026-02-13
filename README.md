# Lexega SQL Releases

Pre-built binaries for [Lexega](https://lexega.com) — pre-execution risk analysis for SQL. Currently supported dialects include Snowflake, PostgreSQL, BigQuery, and Databricks.

## Download

Download the latest release for your platform from the [Releases page](https://github.com/Lexega/releases/releases).

| Platform | Binary |
|----------|--------|
| Linux x64 | `lexega-sql-linux-x64` |
| Linux ARM64 | `lexega-sql-linux-arm64` |
| macOS Intel | `lexega-sql-darwin-x64` |
| macOS Apple Silicon | `lexega-sql-darwin-arm64` |
| Windows x64 | `lexega-sql-windows-x64.exe` |
| Windows ARM64 | `lexega-sql-windows-arm64.exe` |

## Installation

```bash
# Linux / macOS
curl -sSL https://github.com/Lexega/releases/releases/latest/download/lexega-sql-linux-x64 \
  -o lexega-sql && chmod +x lexega-sql
sudo mv lexega-sql /usr/local/bin/

# Verify
lexega-sql --version
```

## CI/CD Installation

```yaml
# GitHub Actions
- name: Install Lexega
  run: |
    curl -sSL https://github.com/Lexega/releases/releases/latest/download/lexega-sql-linux-x64 \
      -o /usr/local/bin/lexega-sql
    chmod +x /usr/local/bin/lexega-sql

- name: Run Lexega
  env:
    LEXEGA_LICENSE_KEY: ${{ secrets.LEXEGA_LICENSE_KEY }}
  run: lexega-sql review origin/main..HEAD . -r
```

## License

A license key is required to use Lexega. [Request a trial](mailto:support@lexega.com).

## Links

- [Documentation](https://lexega.com/docs)
- [Website](https://lexega.com)
