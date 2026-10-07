# Contributing

## Local setup

1. Install the supported Node.js version documented in the README.
2. Run `npm ci`.
3. Copy `.env.example` to `.env.local` and provide development values.
4. Run `npm run dev`.

Never commit `.env.local`, credentials, access tokens, private client information, or production exports.

## Branches

Create one branch per issue. Use a short descriptive name such as:

```text
codex/123-menu-empty-state
```

## Pull requests

- Keep each pull request focused on one issue.
- Link the issue using `Closes #123`.
- Add tests for changed behavior.
- Include before-and-after screenshots for visual changes.
- Run the repository quality checks before requesting review.
- Document follow-up work rather than silently expanding scope.

## Completion standard

Work is complete only when its acceptance criteria pass, checks succeed, documentation is updated, and the pull request is reviewed.

See [Phase 0 and Phase 1 Implementation Plan](docs/PHASE_0_1_IMPLEMENTATION.md) and [GitHub Project Setup and Tracking Guide](docs/GITHUB_PROJECT_TRACKING.md).

