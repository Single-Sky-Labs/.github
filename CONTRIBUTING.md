# Contributing to Single Sky

These rules apply to every Single Sky repo (`single-sky-api`, `single-sky-console`,
`single-sky-cli`). Source ticket: SCRUM-24 (FND-01).

> **Rules by convention.** The org is on GitHub Free, so GitHub cannot block
> direct pushes or unapproved merges on private repos. The local hooks below
> and these rules are the protection. Breaking them on purpose is a serious
> issue, not a shortcut.

## One-time setup (per clone)

```bash
brew install gitleaks              # or see https://github.com/gitleaks/gitleaks
git config core.hooksPath .githooks
```

This turns on two hooks:

| Hook | What it does |
|---|---|
| `pre-commit` | Runs gitleaks on staged changes and blocks the commit if it finds a secret |
| `pre-push` | Refuses pushes to `main` |

## Branches and commits

- Branch from `main`, named `SCRUM-<n>-short-description`, for example `SCRUM-24-repo-setup`.
- Start the PR title with the Jira key: `SCRUM-24 Add repo rules`.
- The Jira key in the branch and PR title puts the work in the Jira Development panel.

## Review rules

| Change | Review needed | Who merges |
|---|---|---|
| Normal code | 1 approval from the **other** engineer. Aim to review the same day. | Author, after approval |
| **Red zone** (list below) | Line-by-line review by the other engineer | Author, after approval. Never self-merge. |
| Trivial: docs, typos, comments only, no red zone | None before merge. Add the `trivial` label. | Author. The other engineer reviews after merge. |

- No direct pushes to `main`. No force-pushes to `main`.
- Keep PRs small: under ~400 changed lines where possible. Small PRs keep review fast.
- AI-assisted code gets the same review as handwritten code. Tick the box in the template so the reviewer knows.

### Red zones

These paths are mapped in each repo's `CODEOWNERS`:

1. Billing, ledger and metering
2. Tenancy and customer isolation, including the SkyPilot AdminPolicy
3. Authentication, sessions and API keys
4. Provider credentials and secrets handling
5. The repo rules themselves: `.github/`, `CODEOWNERS`, `.githooks/`, `.gitleaks.toml`

## Secrets

- Never commit `.env` files, keys, tokens or provider credentials. `.gitignore` covers the usual files, and gitleaks catches the rest.
- Put example config in `.env.example` with fake values.
- **If a secret is pushed:** (1) rotate or revoke it at the provider **immediately**, (2) tell the other engineer, (3) only then clean the history. Deleting the commit does not make the secret safe. Treat it as leaked.
