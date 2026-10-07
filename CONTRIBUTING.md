# Contributing

## Local setup

1. Install the supported Node.js version documented in the README.
2. Run `npm ci`.
3. Copy `.env.example` to `.env.local` and provide development values.
4. Run `npm run dev`.

Never commit `.env.local`, credentials, access tokens, private client information, or production exports.

## Branches

The internship integration branch is `codex/restaurant-mvp`. Do not commit directly to `main` or the integration branch. Create one working branch per issue from the latest integration branch, using a short descriptive name such as:

```text
intern/issue-123-menu-empty-state
```

Open the pull request back to `codex/restaurant-mvp`. See the [Intern Developer Onboarding Guide](docs/INTERN_DEVELOPER_ONBOARDING.md) for the complete workflow.

## Pull requests

- Keep each pull request focused on one issue.
- For intern pull requests targeting `codex/restaurant-mvp`, use `Refs #123` and manually link the issue in the Development sidebar.
- Use `Closes #123` only on a pull request targeting the default `main` branch; GitHub does not process automatic closing keywords for pull requests targeting other branches.
- Add tests for changed behavior.
- Include before-and-after screenshots for visual changes.
- Run the repository quality checks before requesting review.
- Document follow-up work rather than silently expanding scope.

## Completion standard

Work is complete only when its acceptance criteria pass, checks succeed, documentation is updated, and the pull request is reviewed.

See [Phase 0 and Phase 1 Implementation Plan](docs/PHASE_0_1_IMPLEMENTATION.md) and [GitHub Project Setup and Tracking Guide](docs/GITHUB_PROJECT_TRACKING.md).

New contributors should also read the [Intern Developer Onboarding Guide](docs/INTERN_DEVELOPER_ONBOARDING.md) and [GitHub Beginner and Trainer Review Guide](docs/GITHUB_BEGINNER_AND_REVIEW_GUIDE.md).

