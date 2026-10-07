# GitHub Beginner and Trainer Review Guide

This document explains the GitHub platform in the context of the Restaurant Template internship. It is not necessary to learn every GitHub feature before starting. Learn the concepts in the first two sections, then use the project workflow described below.

## 1. Git and GitHub are different

**Git** is the version-control program installed on the developer's computer. It records file changes as commits and supports branches.

**GitHub** is the online collaboration platform that stores the shared Git repository and adds issues, pull requests, reviews, project boards, automation, releases, permissions, and security controls.

```text
Developer computer                    GitHub
------------------                    ------
Working files                         Repository
Local branch          push ────────>  Remote branch
Local commits                         Commit history
                                      Pull request and review
                     pull <─────────  Approved team changes
```

## 2. Essential terms

| Term | Meaning in this project |
|---|---|
| Organization | `Thoorigai-Infotech-Academy`, which owns the repository and teams. |
| Repository | The project files, history, issues, and collaboration settings. |
| Clone | The first download of the repository to a computer. |
| Remote | The online repository; the standard name is `origin`. |
| Branch | An independent line of changes. |
| `main` | The stable branch. It receives only approved release changes. |
| Integration branch | `codex/restaurant-mvp`, where accepted internship work is combined. |
| Issue branch | A short-lived `intern/issue-...` branch for one issue. |
| Commit | A saved checkpoint with an author, time, message, and exact changes. |
| Push | Upload local commits to GitHub. |
| Fetch | Download information about remote changes without changing working files. |
| Pull | Fetch and integrate remote changes into the current local branch. |
| Merge | Combine an accepted branch into another branch. |
| Issue | A trackable requirement, defect, task, decision, or outcome. |
| Sub-issue | A smaller issue linked under a parent issue to divide larger work. |
| Pull request (PR) | A proposal to merge one branch into another, with review and checks. |
| Review | A trainer's comments, requested changes, or approval on a pull request. |
| Project | The planning board that tracks status, priority, phase, and iteration. |
| Milestone | A collection of issues for Phase 0, Phase 1, or the MVP release. |
| Label | A tag describing type, area, priority, or a blocker. |
| Workflow/check | Automation that validates the code, such as lint, tests, or build. |
| Release | A named, versioned delivery based on stable code. |

## 3. Repository navigation

The repository's top navigation contains the main areas:

### Code

Browse files, switch branches, view commit history, and copy the clone address. Always check the branch selector before reading or comparing code.

### Issues

View work items, search them, filter by assignee or label, and record requirements and decisions. The issue is the source of truth for what must be achieved.

### Pull requests

View proposed changes, conversations, automated checks, and approvals. The **Files changed** tab is the main review screen.

### Actions

View automated workflow runs. A green check means the workflow passed; a red cross means the developer must inspect and fix the failure.

### Projects

Open the internship delivery board. Its views provide different perspectives:

- **Intern board**: work grouped by delivery status.
- **Phase roadmap**: the Phase 0 and Phase 1 sequence.
- **Current week**: issues assigned to the active internship week.
- **Blocked work**: issues requiring help or a decision.
- **Release readiness**: final MVP verification work.

### Security

Shows vulnerability and dependency information when the related GitHub security features are enabled. Secrets must never be placed in the repository, even if it is private.

### Insights

Shows activity and contribution information. Use it as context, not as the primary measure of intern performance.

### Settings

Available to administrators. It controls collaborators, teams, branches, rulesets, integrations, and repository behavior.

## 4. How issues and sub-issues should be used

Each implementation issue should describe one reviewable outcome and contain testable acceptance criteria.

Use a parent issue when an outcome requires several independently reviewable pieces. Create sub-issues when:

- Different people can complete pieces independently.
- A piece needs its own branch and pull request.
- The parent is too large to finish in approximately five working days.
- Separate verification or approval is required.

Example:

```text
Parent: Build production reservation capability
  ├── Define request schema and validation
  ├── Create provider adapter
  ├── Implement submission UI states
  └── Add integration and error tests
```

For every issue or sub-issue:

1. Assign an owner.
2. Add the correct milestone and labels.
3. Set Phase, Priority, Effort, Area, and Iteration in the project.
4. Record dependencies.
5. Create a separate issue branch.
6. Link the pull request.
7. Verify all acceptance criteria before closing it.

Do not create a single pull request that claims to close a parent and all sub-issues unless all of them were genuinely implemented and independently verified.

## 5. How the project board should move

```text
Backlog → Ready → In progress → In review → Verify → Done
```

| Status | Meaning | Person responsible for the next action |
|---|---|---|
| Backlog | Planned but not yet prepared for implementation. | Trainer or project owner |
| Ready | Requirements and dependencies are clear. | Assigned intern |
| In progress | Development is actively happening. | Intern |
| In review | Pull request is ready and checks pass. | Trainer reviewer |
| Verify | Code is merged to the integration branch; manual or deployed checks remain. | Intern and trainer |
| Done | Acceptance criteria and verification are complete. | Trainer or project owner |

Use `status:blocked` when work cannot continue. Add `needs:decision`, `needs:design`, or `needs:content` to identify what is missing.

## 6. Recommended branch and release model

```text
issue branch ──PR + trainer approval──> codex/restaurant-mvp
                                             │
                                      release verification
                                             │
                                             └──PR + trainer approval──> main
```

Rules:

- Interns never push directly to `main` or `codex/restaurant-mvp`.
- One issue branch and one pull request should normally represent one issue or sub-issue.
- Intern pull requests use `codex/restaurant-mvp` as the base branch.
- Only accepted and verified work is merged into the integration branch.
- The project owner opens a final release pull request from `codex/restaurant-mvp` to `main`.
- The final PR description uses closing keywords for all accepted issues that should close when the release reaches `main`.

GitHub only interprets automatic issue-closing keywords when a pull request targets the default branch. Therefore, an intern PR to `codex/restaurant-mvp` should use `Refs #<number>` and the Development sidebar to show the relationship. The trainer closes the issue after integration verification, or the final release PR closes it when merged to `main`.

## 7. Trainer approval workflow

### Before development

The trainer:

1. Confirms the issue has clear acceptance criteria.
2. Resolves dependencies and required decisions.
3. Assigns the intern and iteration.
4. Moves the issue to **Ready**.

### During development

The intern:

1. Creates an issue branch from the latest `codex/restaurant-mvp`.
2. Moves the issue to **In progress**.
3. Opens a draft PR early for medium or large changes.
4. Links the PR to the issue using `Refs #<number>` and the Development sidebar.
5. Posts blockers and important decisions on the issue or PR, not only in private chat.

### When requesting review

The intern:

1. Performs a self-review in **Files changed**.
2. Runs lint, tests, and production build.
3. Adds verification notes and screenshots.
4. Marks the PR **Ready for review**.
5. Requests the `Trainers` team.
6. Moves the issue to **In review**.

### Trainer review

The reviewer checks:

- The change matches the issue and does not add unrelated scope.
- Every acceptance criterion has evidence.
- Security, validation, error states, and accessibility are considered.
- Tests cover important behavior and failure paths.
- No secret, client data, build output, or accidental file is committed.
- Naming and structure support the generic business-template direction.
- Lint, test, and build checks pass.
- Documentation is updated where required.

The reviewer then selects one review result:

- **Comment** for non-blocking guidance or questions.
- **Request changes** when the PR must not be merged yet.
- **Approve** when the current commit set is acceptable.

The pull request author cannot approve their own work. An approval should count only from a trainer with Write, Maintain, or Admin access under the repository's enforced review rule.

### Corrections and re-review

The intern pushes corrections to the same branch. The PR updates automatically. If stale approval dismissal is enabled, the previous approval is removed after code-changing commits, and a trainer must review the updated version again.

All review conversations must be resolved before merge. The person who raised a substantive concern should normally confirm its resolution.

### Merge and verification

After approval and passing checks, a trainer—not the intern—merges the pull request into `codex/restaurant-mvp`. Prefer **Squash and merge** for a compact issue-level history unless preserving individual commits has a clear value.

Then:

1. Move the issue to **Verify**.
2. Test the integration branch, including relevant neighboring features.
3. Record verification evidence on the issue.
4. Close the issue and move it to **Done** only after acceptance.

## 8. GitHub configuration for enforced trainer approval

Documentation alone does not prevent accidental direct pushes. Configure a GitHub ruleset or branch protection for both `main` and `codex/restaurant-mvp`.

Repository administrators should open **Settings → Rules → Rulesets** and create branch rules targeting these two branches. If rulesets are unavailable on the organization's plan, use **Settings → Branches → Add classic branch protection rule** for each branch.

Recommended settings:

- Require a pull request before merging.
- Require at least 1 trainer approval. Use 2 approvals for security, authentication, data, or release PRs if trainer availability permits.
- Dismiss stale approvals when new commits are pushed.
- Require approval of the most recent reviewable push.
- Require conversation resolution before merging.
- Require status checks when the CI workflow is available.
- Block force pushes.
- Block branch deletion.
- Apply rules to administrators, or keep bypass access limited to named repository administrators for emergencies.
- Restrict direct pushes to the protected branches.

Create or use a GitHub organization team named `Trainers`. Give the team the repository access level needed for approvals to count. GitHub's required-review rules count approvals from reviewers with Write-level permission or higher.

## 9. Automatic trainer review with CODEOWNERS

A `.github/CODEOWNERS` file can automatically request review from the trainer team. Replace the example team slug with the real slug shown in the GitHub team URL:

This repository uses the verified organization team slug:

```text
# Default owner for every repository file
* @Thoorigai-Infotech-Academy/thoorigai-trainers

# Protect workflow, templates, and ownership rules
/.github/ @Thoorigai-Infotech-Academy/thoorigai-trainers

# Security and authentication changes
/src/context/AuthContext.jsx @Thoorigai-Infotech-Academy/thoorigai-trainers
/src/lib/supabase.js @Thoorigai-Infotech-Academy/thoorigai-trainers
```

The `CODEOWNERS` file must exist on the pull request's base branch. For intern pull requests, add it to `codex/restaurant-mvp`. Enable **Require review from Code Owners** in the rule protecting that branch.

One approval from any matching code owner satisfies the code-owner requirement unless a separate rule requires more total approvals.

## 10. Pull request review checklist for trainers

```markdown
## Scope
- [ ] The PR addresses one issue or sub-issue.
- [ ] The issue is linked and acceptance criteria are current.
- [ ] Unrelated changes are absent or explained.

## Correctness
- [ ] The happy path works.
- [ ] Empty, invalid, loading, and failure states are handled.
- [ ] Existing behavior has not regressed.

## Quality
- [ ] Tests are meaningful and pass.
- [ ] Lint passes.
- [ ] Production build passes.
- [ ] Names and structure are understandable to the next intern.

## Product checks
- [ ] Mobile behavior is acceptable where relevant.
- [ ] Keyboard and screen-reader behavior is acceptable where relevant.
- [ ] Visual evidence is included where relevant.
- [ ] Client-specific content is configuration, not hard-coded shared logic.

## Safety
- [ ] No credentials, personal data, or client-sensitive data are committed.
- [ ] Authentication and validation changes were reviewed carefully.
- [ ] Dependencies and configuration changes are justified.

## Decision
- [ ] Approve
- [ ] Request changes
- [ ] Comment only
```

## 11. Capabilities worth introducing gradually

### GitHub Actions

Automate lint, tests, and builds on each pull request. Make the workflow a required status check only after it is stable.

### Dependabot

Raises dependency update pull requests and vulnerability alerts. Updates still require tests and review.

### Releases and tags

Create a tagged MVP release after `codex/restaurant-mvp` is merged into `main`. Release notes should summarize outcomes and known limitations.

### Discussions

Useful for ideas and broader questions that are not yet approved work. Confirmed implementation work should become an issue.

### Wikis

Useful for long-lived organizational knowledge. Keep code-specific setup beside the code in the repository so it changes through the same review process.

### Security alerts

Use dependency and secret-scanning alerts where available. Treat alerts as work inputs, not automatic permission to merge an update.

### Insights

Useful for understanding activity trends. Do not evaluate an intern by commit count or lines changed; evaluate accepted outcomes, code quality, communication, and learning progress.

## 12. Suggested permissions

| Group | Suggested repository role | Purpose |
|---|---|---|
| Interns | Write | Create branches, push issue work, open PRs, and update issues. |
| Trainers | Maintain or Write | Review, approve, manage issues, and merge accepted PRs. |
| Project owners | Admin | Manage access, rulesets, integrations, and emergency overrides. |

Keep the number of Admin users small. Approval enforcement is strongest when bypass permissions are restricted and documented.

## 13. Official GitHub references

- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [Managing branch protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches)
- [About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Reviewing pull requests](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/reviewing-proposed-changes-in-a-pull-request)
- [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue)

