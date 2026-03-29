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