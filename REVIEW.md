# REVIEW.md

## What this repo is

The Eko Plugin Marketplace hub (GitHub name `eko-claude-plugins`): a distribution index with a Claude marketplace adapter, consumed by users and not deployed (per `AGENTS.md`).
Layout: `registry/` (catalog and schema), `hosts/*/`, `.claude-plugin/` (marketplace manifest), `scripts/`, `docs/` (including ADRs).

## Review priorities (in order)

1. Correctness: `registry/catalog.json`, `.claude-plugin/marketplace.json` and `hosts/` must stay consistent (checked by `scripts/validate-catalog.sh`).
2. Security: the repo is public; no secrets, credentials or personal data (`AGENTS.md`). gitleaks scans the full history in CI.
3. Repo invariants from `AGENTS.md`:
   - distribution only: MUST NOT add `skills/`, `agents/`, `commands/`, `hooks/` logic trees, `mcp-tools/` or vendored plugin source (CI fails on `skills`, `agents`, `commands`, `mcp-tools` directories and on files under `plugins/`);
   - do not add a per-plugin `version` to `marketplace.json`;
   - consumer source pin decisions belong to the ADRs, not to a PR;
   - never commit to `main`; tagging and release are a human gate.

## Sensitive paths

`.github/workflows/**`, `.claude-plugin/**`, `registry/**`, `scripts/validate-catalog.sh`, `docs/adrs/**`, `.github/CODEOWNERS`.

## What NOT to flag

- Style: no linter or formatter config found.
- Unpinned plugin `ref = main`: a documented MVP decision in `AGENTS.md` (ADR), not a defect.

## How to verify

```bash
bash scripts/validate-catalog.sh
```

CI also runs the `gitleaks` workflow (`.github/workflows/gitleaks.yml`, pinned gitleaks 8.30.1, full history).
