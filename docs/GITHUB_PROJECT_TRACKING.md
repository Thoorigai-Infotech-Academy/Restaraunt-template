# GitHub Project Setup and Tracking Guide

This guide converts the Phase 0 and Phase 1 implementation plan into a trackable internship project.

## 1. Repository preparation

1. Push the existing repository to a private GitHub repository until committed credentials have been removed from history and rotated.
2. Protect the default branch.
3. Require pull requests before merging.
4. Require at least one approval.
5. Require the automated `check` workflow after it is introduced.
6. Disable force pushes and branch deletion on the default branch.
7. Add the intern with the minimum repository role needed for branches and pull requests.

## 2. Milestones

Create these milestones:

### Phase 0 — Stabilization

Target: end of internship week 1.

Success means the repository is safe, builds cleanly, has baseline tests, and no longer contains known crash paths.

### Phase 1 — Restaurant MVP

Target: end of internship week 4 or 5.

Success means a pilot restaurant can be configured and deployed without modifying shared components.

### MVP Release

Use this for final release verification, documentation, pilot deployment, and mentor sign-off.

## 3. Labels

Create the following labels:

### Type

- `type:security`
- `type:bug`
- `type:feature`
- `type:refactor`
- `type:test`
- `type:documentation`
- `type:research`

### Area

- `area:core`
- `area:restaurant`
- `area:admin`
- `area:forms`
- `area:seo`
- `area:accessibility`
- `area:performance`
- `area:deployment`

### Priority

- `priority:critical`
- `priority:high`
- `priority:medium`
- `priority:low`

### Workflow

- `status:blocked`
- `needs:decision`
- `needs:design`
- `needs:content`
- `good-first-task`

## 4. Project fields

Create a GitHub Project with these fields:

| Field | Values |
|---|---|
| Status | Backlog, Ready, In progress, In review, Verify, Done |
| Phase | Phase 0, Phase 1, Later |
| Priority | Critical, High, Medium, Low |
| Size | XS, S, M, L, XL |
| Area | Core, Restaurant, Admin, Forms, SEO, Accessibility, Performance, Deployment |
| Iteration | Week 1, Week 2, Week 3, Week 4, Week 5 |
| Owner | Intern or mentor |

Recommended size guide:

- `XS`: less than half a day
- `S`: approximately one day
- `M`: two to three days
- `L`: four to five days
- `XL`: must be split before implementation

## 5. Project views

Create these views:

1. **Intern board:** Board grouped by Status, filtered to the intern.
2. **Phase roadmap:** Table grouped by Phase and sorted by Priority.
3. **Current week:** Board filtered to the active iteration.
4. **Blocked work:** Table filtered to `status:blocked` or `needs:decision`.
5. **Release readiness:** Table containing only High and Critical work not Done.

## 6. Issue backlog

Create one issue for each item below. Copy detailed requirements and acceptance criteria from `docs/PHASE_0_1_IMPLEMENTATION.md` into the corresponding issue.

### Phase 0 issues

| ID | Issue title | Priority | Size | Dependencies |
|---|---|---:|---:|---|
| P0-01 | Remove committed credentials and secure environment configuration | Critical | M | None |
| P0-02 | Restore lint, build, dependency, and quality checks | Critical | M | P0-01 |
| P0-03 | Fix case-sensitive imports and production route handling | High | S | None |
| P0-04 | Add defensive rendering and import validation | Critical | M | P0-02 |
| P0-05 | Make existing admin controls internally consistent | High | M | P0-02 |
| P0-06 | Add baseline automated tests | High | L | P0-04, P0-05 |

### Phase 1 issues

| ID | Issue title | Priority | Size | Dependencies |
|---|---|---:|---:|---|
| P1-01 | Define and validate the client configuration contract | Critical | L | Phase 0 |
| P1-02 | Convert visual branding to theme tokens | High | M | P1-01 |
| P1-03 | Implement configurable section registry and ordering | High | L | P1-01 |
| P1-04 | Separate shared blocks from the restaurant package | High | L | P1-02, P1-03 |
| P1-05A | Define and validate the restaurant menu schema | High | M | P1-01 |
| P1-05B | Build complete menu browsing and filtering | High | L | P1-05A |
| P1-06A | Define reservation form contract and provider adapter | Critical | M | P1-01 |
| P1-06B | Implement production reservation delivery | Critical | L | P1-06A |
| P1-07 | Add configurable SEO and Restaurant structured data | High | M | P1-01 |
| P1-08A | Complete keyboard and screen-reader interaction | High | L | P1-03 |
| P1-08B | Complete mobile and responsive verification | High | M | P1-04, P1-05B |
| P1-09 | Optimize images, fonts, and initial page performance | Medium | L | P1-04 |
| P1-10A | Document new-client configuration and asset replacement | High | M | P1-01, P1-05B |
| P1-10B | Document and verify production deployment | Critical | M | P1-06B, P1-07 |
| P1-11 | Run end-to-end MVP verification and release | Critical | L | All Phase 1 work |

No `XL` issues should enter an iteration. Split them first.

## 7. Suggested internship iterations

### Week 1

- P0-01 through P0-05
- Begin P0-06
- Mentor review of the target architecture

### Week 2

- Complete P0-06
- P1-01
- P1-02
- Begin P1-03

### Week 3

- Complete P1-03
- P1-04
- P1-05A
- Begin P1-05B

### Week 4

- Complete P1-05B
- P1-06A and P1-06B
- P1-07
- Begin accessibility work

### Week 5

- P1-08A and P1-08B
- P1-09
- P1-10A and P1-10B
- P1-11 and release review

This schedule assumes one full-time intern and prompt mentor feedback. Move unfinished work rather than lowering acceptance criteria.

## 8. Issue format

Every issue should contain:

```markdown
## Outcome
What must be true when this issue is complete?

## Context
Why is the work required, and which existing files are involved?

## Scope
- Concrete implementation item
- Concrete implementation item

## Out of scope
- Related work intentionally excluded

## Acceptance criteria
- [ ] Observable, testable result
- [ ] Observable, testable result
- [ ] Tests and documentation updated
- [ ] `npm run check` passes

## Dependencies
- Issue or decision required first

## Verification notes
Commands, screenshots, URLs, or manual checks used to verify completion.
```

## 9. Pull request workflow

1. Move the issue to Ready only when dependencies and required inputs are available.
2. Assign the issue before starting.
3. Create a dedicated branch.
4. Move the issue to In progress.
5. Open a draft pull request early for tasks sized M or L.
6. Link the issue using `Closes #<number>`.
7. Move to In review after automated checks pass.
8. The reviewer verifies the acceptance criteria.
9. Move to Verify for deployed or manual checks.
10. Merge and mark Done only after verification.

## 10. Review responsibilities

### Intern

- Keep the issue updated.
- Raise blockers within one working day.
- Add or update tests.
- Provide screenshots for visual changes.
- Document decisions and limitations.
- Perform a self-review before requesting review.

### Mentor

- Supply decisions and project inputs.
- Review security and architecture tasks directly.
- Review pull requests within an agreed response time.
- Protect the scope from unplanned Phase 2 work.
- Perform weekly release-gate checks.

## 11. Progress reporting

Use a short weekly update:

```markdown
## Completed
- Issues completed this week

## In progress
- Current work and expected completion

## Blocked
- Blocker, impact, and required decision

## Quality
- Test, lint, build, accessibility, or performance status

## Next week
- Planned issues
```

Track completion by accepted issues rather than lines of code or hours spent.

