# Intern Developer Onboarding Guide

This guide is written for a developer who is new to GitHub. Follow the steps in order. If any command shows an error, stop and share the full error message with a trainer before trying unrelated fixes.

## 1. What you will work on

Repository: [Thoorigai-Infotech-Academy/Restaraunt-template](https://github.com/Thoorigai-Infotech-Academy/Restaraunt-template)

Project board: [Restaurant Template MVP — Internship](https://github.com/orgs/Thoorigai-Infotech-Academy/projects/4)

Shared integration branch: `codex/restaurant-mvp`

The shared integration branch contains the combined Phase 0 and Phase 1 work. Do not make commits directly on `main` or `codex/restaurant-mvp`. For every assigned issue, create a small working branch from `codex/restaurant-mvp`, make the change there, and open a pull request back to `codex/restaurant-mvp`.

```text
main
  └── codex/restaurant-mvp
        ├── intern/issue-7-quality-checks
        ├── intern/issue-8-route-fixes
        └── intern/issue-9-defensive-rendering
```

Trainers review each working branch before it is merged into `codex/restaurant-mvp`. When the complete MVP is accepted, trainers merge `codex/restaurant-mvp` into `main` through a final release pull request.

## 2. Accounts and access

Ask a trainer to confirm all of the following before setup:

- You have a GitHub account.
- You accepted the invitation to the `Thoorigai-Infotech-Academy` organization.
- You can open the repository and project board links above.
- You have Write access to the repository so you can push a working branch and open a pull request.
- Two-factor authentication is configured if the organization requires it.
- You know which issue is assigned to you. Do not start an unassigned issue.

Never share your GitHub password, personal access token, Supabase key, `.env.local` file, or one-time password with another person.

## 3. Install the development tools

Install these tools on the development machine:

1. [Git](https://git-scm.com/downloads)
2. [Node.js](https://nodejs.org/) version 22 LTS. This repository requires Node.js `20.19` or newer; Node 22 LTS is the recommended choice.
3. [Visual Studio Code](https://code.visualstudio.com/)
4. A modern browser such as Chrome, Edge, or Firefox.

During Git installation on Windows, the default options are suitable. Open PowerShell or the VS Code terminal and check the installation:

```powershell
git --version
node --version
npm --version
```

Expected result:

- Each command prints a version rather than an error.
- The Node.js version is at least `20.19.0`.

## 4. Configure Git once

Use the same name and email address connected to your GitHub account:

```powershell
git config --global user.name "Your Full Name"
git config --global user.email "your-github-email@example.com"
git config --global init.defaultBranch main
```

Check the saved values:

```powershell
git config --global --list
```

Use your own details. Do not copy another developer's name or email.

## 5. Download the project for the first time

Choose a normal development folder. Do not clone the project inside OneDrive if file synchronization causes Git problems.

```powershell
cd C:\Projects
git clone --branch codex/restaurant-mvp https://github.com/Thoorigai-Infotech-Academy/Restaraunt-template.git
cd Restaraunt-template
```

GitHub may open a browser and ask you to sign in. Complete the sign-in using your own GitHub account.

Confirm the branch and repository state:

```powershell
git status
git branch --show-current
git remote -v
```

Expected result:

- Current branch: `codex/restaurant-mvp`
- Working tree: clean
- Remote named `origin` points to `Thoorigai-Infotech-Academy/Restaraunt-template`

Open the folder in VS Code:

```powershell
code .
```

## 6. Install and run the application

Install the exact dependency versions recorded in `package-lock.json`:

```powershell
npm ci
```

Create the local environment file:

```powershell
Copy-Item .env.example .env.local
```

Ask a trainer for development-only Supabase values and place them in `.env.local`:

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=your-development-publishable-key
```

Do not add `.env.local` to Git. Confirm that Git ignores it:

```powershell
git status --short
```

Start the site:

```powershell
npm run dev
```

Open the local address displayed in the terminal, normally `http://localhost:5173`.

Before beginning assigned work, run the existing quality checks:

```powershell
npm run lint
npm run build
```

The project will later add a combined `npm run check` command. Until that issue is completed, run both commands above.

## 7. Understand an assigned issue

On the project board:

1. Open the issue assigned to you.
2. Read the Outcome, Scope, Out of scope, Acceptance criteria, Dependencies, and Verification notes.
3. Confirm that dependencies are complete.
4. Ask a trainer about anything unclear before writing code.
5. Move the item from **Ready** to **In progress** when you actually begin.

An issue is the agreed unit of work. Do not silently add extra features. Create or request a follow-up issue for additional work.

## 8. Create one working branch per issue

The following example uses issue `#7`. Replace the number and description with the assigned issue.

First, update your local integration branch:

```powershell
git switch codex/restaurant-mvp
git pull --ff-only origin codex/restaurant-mvp
```

Create the issue branch:

```powershell
git switch -c intern/issue-7-quality-checks
```

Recommended branch format:

```text
intern/issue-<issue-number>-<short-description>
```

Good examples:

- `intern/issue-7-quality-checks`
- `intern/issue-8-route-fixes`
- `intern/issue-17-keyboard-accessibility`

Avoid names such as `my-branch`, `test`, `new`, or `final-final`.

## 9. Make and verify the code change

While working:

1. Change only what the issue requires.
2. Check the site in the browser regularly.
3. Add or update tests when behavior changes.
4. Do not commit generated files, passwords, `.env.local`, exported customer data, or editor cache files.
5. Add a comment to the issue if you are blocked for more than one working day.

Before committing, review your work:

```powershell
git status
git diff
npm run lint
npm run build
```

When the repository gains tests or `npm run check`, run those commands as well.

For a visual change, capture before-and-after screenshots. For a bug fix, record simple reproduction and verification steps.

## 10. Commit the change

Add only the files that belong to the issue. Listing files explicitly helps prevent accidental commits:

```powershell
git add src\path\changed-file.jsx src\styles\changed-file.css
git status
```

Create a short commit message that describes the outcome and mentions the issue:

```powershell
git commit -m "fix: restore production route handling (#8)"
```

Useful prefixes:

- `fix:` for a defect
- `feat:` for a feature
- `refactor:` for code restructuring without a behavior change
- `test:` for tests
- `docs:` for documentation
- `chore:` for tooling or maintenance

It is normal to have several small, meaningful commits on one issue branch.

## 11. Sync safely when other work has changed

Before pushing or updating a pull request:

```powershell
git fetch origin
git switch codex/restaurant-mvp
git pull --ff-only origin codex/restaurant-mvp
git switch intern/issue-7-quality-checks
git merge codex/restaurant-mvp
```

If Git reports a merge conflict:

1. Do not delete files or use `git reset --hard`.
2. Open the conflicting files in VS Code.
3. Ask a trainer to help if you do not understand both changes.
4. Resolve each conflict, test the result, and then run:

```powershell
git add path\to\resolved-file
git commit
```

## 12. Push the issue branch

The first push sets the remote tracking branch:

```powershell
git push -u origin intern/issue-7-quality-checks
```

Later pushes use:

```powershell
git push
```

Never force-push unless a trainer specifically guides you through the situation.

## 13. Open a pull request and connect it to the issue

On GitHub:

1. Open the repository.
2. Select **Pull requests** and then **New pull request**.
3. Set the base branch to `codex/restaurant-mvp`.
4. Set the compare branch to your `intern/issue-...` branch.
5. Confirm that only the intended files and commits are shown.
6. Complete every section of the pull request template.
7. Put `Refs #7` in the Summary, replacing `7` with the issue number.
8. In the right sidebar, request the `Trainers` team as reviewers.
9. Add screenshots for visual changes.
10. Create a **Draft pull request** if the work is not ready. Select **Ready for review** only after self-review and all available checks pass.

Important: these intern pull requests target `codex/restaurant-mvp`, not the default `main` branch. GitHub closing keywords such as `Closes #7` only link and automatically close an issue when the pull request targets the default branch. Therefore, use `Refs #7` and manually link the issue in the pull request's **Development** section. A trainer closes the issue after the work is merged and verified.

Move the project item to **In review** when the pull request is ready.

## 14. Respond to trainer review

A trainer may:

- **Comment**: advice or a question; it does not necessarily block merging.
- **Request changes**: the pull request is not ready to merge.
- **Approve**: the trainer accepts the reviewed version.

When changes are requested:

1. Read every comment.
2. Ask for clarification if needed.
3. Make the change on the same issue branch.
4. Run the checks again.
5. Commit and push normally.
6. Reply briefly to resolved conversations.
7. Re-request review.

```powershell
git add path\to\changed-file
git commit -m "fix: address reservation validation review (#6)"
git push
```

Do not open a second pull request for review corrections. The existing pull request updates automatically after a push.

## 15. After approval and merge

The trainer merges the pull request into `codex/restaurant-mvp`. Then:

1. Move the issue to **Verify** if manual or deployed verification remains.
2. Add verification evidence to the issue.
3. After trainer acceptance, close the issue and move it to **Done**.
4. Update your local integration branch.
5. Delete the completed local branch.

```powershell
git switch codex/restaurant-mvp
git pull --ff-only origin codex/restaurant-mvp
git branch -d intern/issue-7-quality-checks
```

The trainer may delete the remote issue branch when merging the pull request.

## 16. Daily routine

At the start of the day:

```powershell
git status
git fetch origin
git switch codex/restaurant-mvp
git pull --ff-only origin codex/restaurant-mvp
git switch your-issue-branch
git merge codex/restaurant-mvp
```

Before ending the day:

```powershell
git status
git add <only-the-required-files>
git commit -m "type: clear progress description (#issue-number)"
git push
```

Also update the issue with what was completed, what remains, and any blocker.

## 17. Common problems

### `git` or `node` is not recognized

Close and reopen VS Code or PowerShell after installation. If it still fails, ask a trainer to verify the installation and system PATH.

### `npm ci` fails

Check the Node.js version with `node --version`, confirm internet access, and share the full error with a trainer. Do not delete `package-lock.json`.

### The site cannot sign in to Admin

Check that `.env.local` exists, variable names match `.env.example`, and development Supabase values were supplied by a trainer. Never paste the values into an issue or pull request.

### Git says the branch is behind

Fetch and merge the integration branch using the commands in section 11. Do not force-push.

### A wrong file was staged

Unstage it without deleting the file:

```powershell
git restore --staged path\to\file
```

### A secret was committed

Stop immediately. Do not merely delete it in another commit. Tell a trainer so the credential can be rotated and Git history can be cleaned safely.

## 18. Help request template

Use this in the issue when requesting help:

```markdown
## Blocker
What I was trying to do:

## What happened
The exact error or unexpected behavior:

## What I checked
- Command or test already attempted
- Relevant file or page

## Help needed
The decision, access, or technical guidance required:
```

