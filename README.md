# pgspectre-action

GitHub Action for [pgspectre](https://github.com/ppiankov/pgspectre) — PostgreSQL schema and usage auditor. Detects drift between code and database.

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `command` | Subcommand: `audit`, `check`, or `scan` | `audit` |
| `version` | pgspectre version to install | `latest` |
| `db-url` | PostgreSQL connection URL | |
| `repo-path` | Path to code repository (for `check`/`scan`) | `.` |
| `format` | Output format: `text`, `json`, `sarif` | `text` |
| `fail-on` | Exit 2 if findings match (e.g., `MISSING_TABLE,high`) | |
| `min-severity` | Minimum severity to report | |
| `baseline` | Path to baseline file | |
| `args` | Additional CLI arguments | |
| `upload-sarif` | Upload SARIF to GitHub Security tab | `true` |

## Usage

### Audit (cluster-only analysis)

```yaml
- uses: ppiankov/pgspectre-action@v1
  with:
    command: audit
    db-url: ${{ secrets.DATABASE_URL }}
    format: text
    min-severity: medium
```

### Check (code + cluster drift detection)

```yaml
- uses: ppiankov/pgspectre-action@v1
  with:
    command: check
    db-url: ${{ secrets.DATABASE_URL }}
    repo-path: .
    fail-on: MISSING_TABLE,MISSING_COLUMN
```

### Scan (code-only, no database)

```yaml
- uses: ppiankov/pgspectre-action@v1
  with:
    command: scan
    repo-path: .
    format: json
```

### SARIF upload to GitHub Security tab

```yaml
- uses: ppiankov/pgspectre-action@v1
  with:
    command: audit
    db-url: ${{ secrets.DATABASE_URL }}
    format: sarif
```

When `format: sarif`, results are automatically uploaded to the GitHub Security tab (disable with `upload-sarif: false`).

### With baseline

```yaml
- uses: ppiankov/pgspectre-action@v1
  with:
    command: audit
    db-url: ${{ secrets.DATABASE_URL }}
    baseline: .pgspectre-baseline.json
```

## License

MIT
