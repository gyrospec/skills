---
name: gyro-discovery
description: Interview in rounds and record a feature's facts, decisions, assumptions, and open questions.
disable-model-invocation: true
---

# Discovery

1. Call the Skill tool with "gyro". Use its script resolution, Node 20+ check, language rules, and approval etiquette throughout. Reply in the user's language and keep convention keywords English.
2. If a slug is supplied, run `gyro status <slug> --json` before work. Resume its discovery log if the ticket exists. If no slug is given, run `gyro status --json` to inspect existing tickets, then ask for missing title, slug, and size. A missing workspace or new slug gives exit 2 naming `/gyro-discovery`; this is expected during first discovery: confirm the product repository root and missing inputs, run `gyro init <slug> --title "<title>" --size <size>`, then run `gyro status <slug> --json`. Do not overwrite an existing ticket or restart a completed phase automatically. If `next` points past discovery, explain the current state and limit any requested discovery revision to the user's scope.
3. Dig for context before the first question. Read any existing `.gyro/<slug>/discovery.md`, the repository glossary, and every spec, document, or file the user points to, plus the code and docs the feature will touch. Record what they settle as Facts with their source, and never ask what they already answer. When domain terms are ambiguous, Call the Skill tool with "domain-modeling" to challenge terms and resolve examples with the user. Keep unresolved meanings as open questions.
4. Call the Skill tool with "grilling". Interview in rounds and cover the dimensions required by the selected size, asking as few questions as that coverage allows:
   - **Why first.** If the user has not said why they want this, the first round asks that alone, openly, with no recommended answer and no example answers to pick from.
   - **Ask only what changes what gets built.** A question earns its place when its answer would change the product and nothing read in step 3 answers it. Where you can make the call yourself, state it as a proposal the user can correct instead of a question, and record it as an Assumption until the user confirms it.
   - **Never ask about process.** How to write things up, where files go, whether to look further, and how to show something are your calls; make them and say what you decided.
   - **Choices come with options.** When a question is a choice you can name, list the options, recommend one, and word the question so "yes" accepts the recommendation. Number every question in the round.

   Do not infer agreement with recommendations or proposals, and do not turn "I don't know" into a decision. A proposal becomes a Decision only when the user confirms it, one by one or by explicitly accepting the batch.

| Size | Required dimensions |
|---|---|
| Small | Problem & Outcome, Scope & Boundaries, Business Rules, Acceptance Criteria |
| Medium | Small, plus Personas & Authorization, User Journey, Data & Integrations, Failure & Recovery |
| Large | All 10 dimensions |

The ten dimensions are:

1. Problem & Outcome: the pain, and exactly one core objective in one sentence; its success metrics; and the current baseline of each metric (the value today, with where it comes from). When several objectives come up, ask the user to choose the core one. An unknown baseline is recorded as an open question, and the user confirms whether it blocks the PRD.
2. Personas & Authorization: actors, permissions, ownership, and access boundaries.
3. Scope & Boundaries: included behavior, exclusions, and neighboring responsibilities.
4. User Journey: entry points, steps, alternate paths, and completion.
5. Business Rules: invariants, transitions, validations, and exceptions.
6. Data & Integrations: sources, contracts, storage, and external dependencies.
7. Failure & Recovery: expected failures, retries, partial progress, and recovery.
8. Acceptance Criteria: observable examples of success and rejection.
9. Constraints & Dependencies: technical, operational, timing, and prerequisite constraints.
10. Rollout & Migration: compatibility, migration, release, and rollback.

Non-required dimensions may be skipped. A required dimension may be `not-applicable` only with a recorded reason confirmed by the user.

5. After each round append every answer to `.gyro/<slug>/discovery.md`, organized under `Facts`, `Decisions`, `Assumptions`, and `Open questions`. Preserve the question, answer, dimension, and round so context survives a fresh session. Separate user decisions from recommendations and assumptions. Record contradictions rather than resolving them silently. Ask the user to confirm criticality (`blocker`, `major`, `minor`) and which unresolved questions block the PRD. Mark each open question's blocking status explicitly.
6. Continue rounds until every required dimension is covered or confirmed `not-applicable`, no blocker remains open or conflicted, and the user explicitly agrees discovery is complete. Record that agreement in the discovery log. Discovery has no approval gate; do not run `gyro approve` for it. If a blocker cannot be resolved, name it and stop with discovery still incomplete.
7. Run `gyro status <slug> --json`. Tell the user the next step is `/gyro-prd <slug>` and stop. Do not invoke it.
