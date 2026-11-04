# Commits

The rules for every commit a Gyrospec skill or its subagent makes. They combine [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) with the writing policy of [meaningful-commit](https://github.com/brahmantiodiaz/meaningful-commit-skill). Commit only when the calling skill or the user authorizes it.

## Stage

1. Run `git status --short --branch`, then `git diff --cached --stat` and `git diff --cached`. The message describes only what is staged.
2. Stage the paths in scope by name. Never use `git add -A` or `git add .`.
3. Make one commit per logical change. Split unrelated changes into separate commits instead of writing a generic message.

## Message

```text
type(scope)!: Capitalized imperative description

What changed and why it was needed, wrapped at about 72 columns.

BREAKING CHANGE: Compatibility impact and migration steps.
Refs: #123
```

- **Type:** `feat` (new capability), `fix` (repaired behavior), `docs`, `style` (formatting only), `refactor` (structure without behavior change), `perf`, `test`, `build`, `ci`, `chore`, `ops`, or `revert`.
- **Scope:** optional; a domain or module name, kept consistent within the repository.
- **Description:** imperative and capitalized, with no trailing period. Aim for 50 characters and never exceed 72 for the whole header. Empty phrases such as "Update code" or "Fix bug" say nothing.
- **Body:** for any non-trivial change, after a blank line: what changed and why, plus trade-offs or migration behavior that matter. Never claim a test or outcome that did not run.
- **Breaking change:** `!` after the type or scope, a `BREAKING CHANGE:` footer, or both, with the migration spelled out.

Good: `fix(lead): Prevent duplicate lead submissions`. Not useful: `fix: fix`, `chore: changes`, `feat: Added some new things`.

Merge and revert messages that Git generates are fine as they are.

## Commit

- Write a multiline message to a file outside the repository and run `git commit -F <file>`.
- No AI attribution anywhere: no `Co-Authored-By` trailer naming an agent, no "Generated with" line, and no agent named as author. The same holds for PR titles, PR bodies, and PR comments.
- Never bypass a hook with `--no-verify`. When a hook rejects the commit, fix the message or the change. Never amend a pushed commit or force-push to satisfy these rules.
- Report each commit's hash and subject.
