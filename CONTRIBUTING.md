# Contributing to evnx-test

Thank you for helping improve the evnx test suite. This repo has two goals:
fixture-driven automated CI and a playground for new users. Contributions
to either are welcome.

---

## Adding a new fixture

1. Create a `.env` file in `fixtures/` named for the scenario you are testing
   (e.g. `empty-values.env`, `utf8-values.env`)
2. Add a corresponding `.env.example` if the test involves `diff` or `sync`
3. Keep fake secrets realistic (real key patterns, obviously fake values)
   so evnx detection fires correctly without leaking real credentials
4. Add a test step in the relevant workflow under `.github/workflows/`
5. Run the command locally, capture the output, and commit it to `outputs/`:

```bash
evnx scan --path fixtures/my-new.env --format json --exit-zero > outputs/scan-my-new.json
git add fixtures/ outputs/
git commit -m "feat: add my-new fixture and snapshot"
```

---

## Adding a new workflow

1. Create `.github/workflows/test-<name>.yml`
2. Follow the existing pattern: `checkout → install evnx → run steps`
3. Add a badge row to `README.md`:

```markdown
| My new workflow | [![test-name](https://github.com/urwithajit9/evnx-test/actions/workflows/test-name.yml/badge.svg)](https://github.com/urwithajit9/evnx-test/actions/workflows/test-name.yml) |
```

4. Keep each job focused on one command or one scenario — avoid mega-jobs

---

## Updating snapshots

When evnx output changes intentionally (new version, new finding format),
regenerate the affected snapshots locally:

```bash
# Regenerate all snapshots in one pass
evnx scan  --path fixtures/secrets.env   --format json   --exit-zero > outputs/scan-json.json
evnx scan  --path fixtures/secrets.env   --format pretty --exit-zero > outputs/scan-pretty.txt
evnx scan  --path fixtures/secrets.env   --format sarif  --exit-zero > outputs/scan-sarif.json

evnx validate --file fixtures/placeholders.env --format json   --exit-zero > outputs/validate-json.json
evnx validate --file fixtures/placeholders.env --format pretty --exit-zero > outputs/validate-pretty.txt

cp fixtures/drift.env .env && cp fixtures/drift.env.example .env.example
evnx diff --format json --exit-zero > outputs/diff-output.json || true
rm .env .env.example

evnx convert --to json           --file fixtures/clean.env > outputs/convert-json.json
evnx convert --to yaml           --file fixtures/clean.env > outputs/convert-yaml.yaml
evnx convert --to kubernetes     --file fixtures/clean.env > outputs/convert-kubernetes.yaml
evnx convert --to terraform      --file fixtures/clean.env > outputs/convert-terraform.tfvars
evnx convert --to github-actions --file fixtures/clean.env > outputs/convert-github-actions.yaml

evnx doctor --exit-zero > outputs/doctor-output.txt

git add outputs/
git commit -m "chore: update snapshots for evnx vX.Y.Z"
```

---

## Playground files

Files in `playground/` are intentionally broken (fake secrets, placeholders,
missing keys). Keep them that way — they are the "live demo" for new users.

If you add a new playground scenario, update `playground/README.md` with a
row in the "Try it" table explaining what to edit and what evnx will catch.

---

## Commit style

```
feat:  add <scenario> fixture
fix:   correct <workflow> step
chore: update snapshots for evnx vX.Y.Z
docs:  update README badge / link
```

---

## Questions

Open an issue on [urwithajit9/evnx](https://github.com/urwithajit9/evnx)
or start a discussion on this repo.