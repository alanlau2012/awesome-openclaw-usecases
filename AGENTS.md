# AGENTS.md

## Cursor Cloud specific instructions

This repository is **documentation-only** (Awesome OpenClaw Use Cases). There is no application runtime, package manager, or database to install.

### What this repo is

- Markdown index: `README.md`, `README_CN.md`
- 29 use-case guides under `usecases/`
- CI: `.github/workflows/update-badge.yml` auto-updates the use-case count badge on push to `main`

### Services

| Service | Required locally? | Notes |
|---------|-------------------|-------|
| *(none)* | No | No servers or APIs are defined in this repo |
| GitHub Actions | Optional | Badge update runs on GitHub when `usecases/*.md` changes |

To run the **OpenClaw workflows** described in the docs, use the external [OpenClaw](https://github.com/openclaw/openclaw) project and per-use-case integrations — not this repository.

### Validation (replaces lint/test for this repo)

Run from repo root:

```bash
# Badge count matches files on disk
COUNT=$(ls usecases/*.md | wc -l | tr -d ' ')
grep -q "usecases-${COUNT}-blue" README.md && echo "Badge OK"

# Simulate CI badge update (dry run — restore README after)
cp README.md /tmp/README.bak
sed -i "s|usecases-[0-9]*-blue|usecases-${COUNT}-blue|" README.md
diff -q /tmp/README.bak README.md || echo "Badge would be updated"
mv /tmp/README.bak README.md
```

Optional: serve docs locally for preview:

```bash
python3 -m http.server 8080
# Open http://localhost:8080/README.md
```

### Contributing workflow

See `CONTRIBUTING.md`: add a markdown file in `usecases/`, add a row to the category table in `README.md`, open a PR. The CI workflow will update the badge count when merged to `main`.

### Gotchas

- `phone-based-personal-assistant.md` appears twice in the README table (duplicate row, not a broken link).
- No pre-commit hooks, linters, or test suites are configured in this repo.
