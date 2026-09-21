<p align="center">
  <img src="https://raw.githubusercontent.com/coccinella-labs/proof/main/.github/assets/thumbnail.png" alt="proof" width="100%">
</p>

# Proof

Deterministic PR body review. Checks structure, word count, issue links, file anchors, and house style. No model, no API keys beyond the GitHub token.

## What it checks

- Title matches `[scope] message`.
- Required sections present (default `## Changes`, `## Validation`).
- Body word count within range (default 30-400).
- `Closes`/`Fixes`/`Resolves #N` references point at real open issues (skipped once the PR itself is closed, where a closed target means the link served its purpose).
- `file:line` references resolve against the PR head (file exists, line in range).
- House style (no em dashes by default).

## Usage

```yaml
- uses: coccinella-labs/proof@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| github-token | GitHub token for API access | `github.token` |
| pr-number | PR to review (defaults to the event) | - |
| min-words | Minimum body word count | `30` |
| max-words | Maximum body word count | `400` |
| require-sections | Comma-separated required headings | `## Changes,## Validation` |
| check-anchors | Verify file:line references | `true` |
| check-em-dashes | Flag em dash characters | `true` |
| comment | Post report as PR comment: `always`, `on-failure`, `never`. Reruns update the existing report comment instead of stacking new ones. | `on-failure` |

## Outputs

| Output | Description |
|--------|-------------|
| passed | Whether all checks passed |
| summary | Markdown report of the checks |

## Example

```yaml
name: Proof
on:
  pull_request:
    types: [opened, edited, synchronize]

jobs:
  proof:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: coccinella-labs/proof@v1
```

## License

MIT
