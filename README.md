# evnx-test

> Test suite and playground for [evnx](https://evnx.dev) — the Rust CLI for `.env` lifecycle management — and the [evnx GitHub Action](https://github.com/marketplace/actions/evnx-env-security-validation).


# evnx playground

No installation needed. Fork this repo, edit any `.env` file below,
push your change, and watch GitHub Actions run evnx live against your file.

## Try it

| File | What to test |
|---|---|
| `try-scan.env` | Edit or add secrets — watch evnx catch them |
| `try-validate.env` | Add placeholder values — watch validation fail |
| `try-diff.env` | Remove a key — watch drift detection trigger |

## What you will see in Actions

- Inline annotations on your `.env` file showing exactly which line has an issue
- Exit code 1 if findings are detected (the job turns red)
- JSON output in the step log with structured findings

## Install evnx locally

```bash
curl -fsSL https://dotenv.space/install.sh | bash
evnx --version
evnx doctor
```

Full docs: https://evnx.dev

---

## CI status

| Workflow | Status |
|---|---|
| All commands | [![test-commands](https://github.com/urwithajit9/evnx-test/actions/workflows/test-commands.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-commands.yml) |
| Marketplace action | [![test-action](https://github.com/urwithajit9/evnx-test/actions/workflows/test-action.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-action.yml) |
| Output formats | [![test-formats](https://github.com/urwithajit9/evnx-test/actions/workflows/test-formats.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-formats.yml) |
| OS matrix | [![test-matrix](https://github.com/urwithajit9/evnx-test/actions/workflows/test-matrix.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-matrix.yml) |
| Snapshots | [![test-snapshots](https://github.com/urwithajit9/evnx-test/actions/workflows/test-snapshots.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-snapshots.yml) |

---

## Playground — try evnx without installing anything

1. **Fork** this repo
2. **Edit** any file in `playground/` (add a fake secret, a placeholder, remove a key)
3. **Push** your change
4. Go to the **Actions tab** — watch evnx run live against your file

No installation. No configuration. Results appear as inline annotations on your files.

See [playground/README.md](playground/README.md) for the guided tour.

---

## Repo structure

| Folder | Purpose |
|---|---|
| `fixtures/` | Curated `.env` files, one per test scenario |
| `outputs/` | Committed snapshots of expected command output (seeded by CI on first run) |
| `playground/` | Pre-seeded files designed for new users to experiment with |
| `.github/workflows/` | Automated CI — commands, action, formats, matrix, snapshots |

### Fixture files

| File | Tests |
|---|---|
| `clean.env` + `clean.env.example` | Zero-findings baseline — every command should pass |
| `secrets.env` | AWS, Stripe, GitHub, OpenAI, Anthropic key patterns (fake values) |
| `placeholders.env` | `CHANGE_ME`, `YOUR_KEY_HERE`, `<your-api-key>` |
| `boolean-trap.env` | `DEBUG="False"` — string value is truthy in Python / Node |
| `weak-secret.env` | `SECRET_KEY=password123`, `JWT_SECRET=secret` |
| `localhost-prod.env` | `DB_HOST=localhost` in a production context |
| `drift.env` + `drift.env.example` | `.env` is missing `REDIS_URL` and `STRIPE_KEY` |
| `multi-service.env` | Postgres + Redis + Stripe + OpenAI combined |

### Workflows

| Workflow | What it covers |
|---|---|
| `test-commands.yml` | All 6 commands (`scan`, `validate`, `diff`, `doctor`, `convert`, `sync`) against fixture files |
| `test-action.yml` | End-to-end Marketplace action test, including SARIF Security tab upload |
| `test-formats.yml` | All output formats: `pretty`, `json`, `sarif`, `github`, `github-actions`, all 14 convert targets |
| `test-matrix.yml` | ubuntu-latest, macos-latest, windows-latest |
| `test-snapshots.yml` | Compares live evnx output against committed `outputs/` snapshots — catches regressions |

---

## Links

| | |
|---|---|
| Homepage | https://evnx.dev |
| Main repo | https://github.com/urwithajit9/evnx |
| GitHub Action | https://github.com/marketplace/actions/evnx-env-security-validation |
| npm | `npm install -g @evnx/cli` |
| PyPI | `pip install evnx` |
| Install | `curl -fsSL https://dotenv.space/install.sh \| bash` |

---

## License

MIT License — Copyright (c) 2026 Ajit Kumar