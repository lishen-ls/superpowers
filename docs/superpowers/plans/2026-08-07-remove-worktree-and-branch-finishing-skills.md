# Remove Worktree and Branch Finishing Skills Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Remove `using-git-worktrees` and `finishing-a-development-branch` while keeping plan execution workflows complete and free of missing-skill invocations.

**Architecture:** Skill discovery remains directory-based. The two skill directories are removed, and their callers are changed to execute in the current working directory and finish by verifying, reviewing, cleaning plan-local scratch data, and reporting status without branch integration actions.

**Tech Stack:** Markdown skill definitions, Bash behavior tests, repository documentation.

## Global Constraints

- Plan execution runs in the current working directory.
- Superpowers must not create, verify, own, or clean up Git worktrees.
- Completion must report verification and review status without automatically merging, creating a pull request, deleting a branch, or cleaning a worktree.
- Historical release notes and completed historical design or plan documents remain unchanged.
- Do not create commits as part of this implementation unless the human partner explicitly requests one.

---

### Task 1: Change Active Workflow Expectations

**Files:**
- Modify: `tests/claude-code/test-subagent-driven-development.sh:153-175`
- Delete: `tests/claude-code/test-worktree-native-preference.sh`
- Delete: `tests/claude-code/test-worktree-path-policy.sh`
- Modify: `tests/claude-code/run-skill-tests.sh:75-80`
- Modify: `docs/testing.md:10-20`

**Interfaces:**
- Consumes: the approved behavior in `docs/superpowers/specs/2026-08-07-remove-worktree-and-branch-finishing-skills-design.md`.
- Produces: active tests and test documentation that no longer require either deleted skill.

- [ ] **Step 1: Replace obsolete SDD expectations**

Replace the worktree prerequisite and main-branch tests with checks that SDD uses the current working directory and finishes by reporting verified/reviewed status without invoking a branch-finishing skill.

- [ ] **Step 2: Remove tests owned exclusively by the deleted skills**

Delete `test-worktree-native-preference.sh` and `test-worktree-path-policy.sh` in full.

- [ ] **Step 3: Update the Claude Code test runner**

Remove `test-worktree-path-policy.sh` from the `tests=(...)` array while retaining `test-sdd-workspace.sh` because plan-local SDD scratch isolation remains supported even when the user happens to be in an externally created Git worktree.

- [ ] **Step 4: Update test documentation**

Remove the `test-worktree-native-preference.sh` inventory entry from `docs/testing.md`; retain SDD tests and helpers.

- [ ] **Step 5: Verify deleted test references are gone**

Run:

```bash
rg -n 'test-worktree-native-preference|test-worktree-path-policy' tests docs/testing.md
```

Expected: no matches.

### Task 2: Remove Worktree and Branch-Finishing Calls from Skills

**Files:**
- Modify: `skills/executing-plans/SKILL.md`
- Modify: `skills/writing-plans/SKILL.md`
- Modify: `skills/subagent-driven-development/SKILL.md`

**Interfaces:**
- Consumes: existing plan execution, SDD ledger, review-package, and final-review workflows.
- Produces: complete execution workflows whose entry point is the current directory and whose terminal state is a completion report.

- [ ] **Step 1: Simplify executing-plans setup**

Remove the required `using-git-worktrees` setup step. Begin plan review by reading the plan in the current working directory.

- [ ] **Step 2: Replace executing-plans completion behavior**

Replace the `finishing-a-development-branch` invocation with explicit instructions to run the plan's final verification, summarize completed tasks and verification results, report blockers or residual concerns, and stop without merging, creating a pull request, deleting branches, or cleaning worktrees.

- [ ] **Step 3: Remove writing-plans worktree context**

Delete the sentence at `skills/writing-plans/SKILL.md:16` that says isolated worktrees should be created through `using-git-worktrees`.

- [ ] **Step 4: Update the SDD flowchart**

Rename the setup node from worktree setup to current-directory setup. Replace the terminal `finishing-a-development-branch` node and edge with a node that reports completion, verification, review status, and residual concerns.

- [ ] **Step 5: Update SDD setup prose**

Remove the worktree requirement and the main/master branch consent rule. State that execution uses the current working directory and does not create or switch workspaces.

- [ ] **Step 6: Update SDD final-review and finish prose**

Replace the statement that residual findings surface through `finishing-a-development-branch`. Keep deletion of the plan-local `.superpowers/sdd/<plan>/` scratch workspace after final review, then require a direct report containing completed tasks, verification evidence, final-review outcome, parked findings, and blockers. Explicitly prohibit automatic merge, pull-request creation, branch deletion, and worktree cleanup.

- [ ] **Step 7: Update the SDD example**

Replace `[Setup: worktree verified]` with current-directory setup and replace the final `finishing-a-development-branch` message with a concise completion report.

- [ ] **Step 8: Verify active skill references are gone**

Run:

```bash
rg -n 'using-git-worktrees|finishing-a-development-branch' \
  skills/executing-plans \
  skills/writing-plans \
  skills/subagent-driven-development
```

Expected: no matches.

### Task 3: Delete Skills and Update the Public Inventory

**Files:**
- Delete: `skills/using-git-worktrees/SKILL.md`
- Delete: `skills/finishing-a-development-branch/SKILL.md`
- Modify: `README.md:196-238`

**Interfaces:**
- Consumes: directory-based skill discovery used by all supported harnesses.
- Produces: a bundled skill set and current documentation containing neither deleted skill.

- [ ] **Step 1: Delete both skill directories**

Delete the tracked `SKILL.md` file in each target directory so Git removes the now-empty directories.

- [ ] **Step 2: Update the Basic Workflow**

Remove the worktree step, renumber the remaining workflow, and change the final step to describe verification, review, and completion reporting rather than branch finishing.

- [ ] **Step 3: Update the Skills Library inventory**

Remove both skill entries from the Collaboration section.

- [ ] **Step 4: Confirm directory-based discovery no longer exposes the skills**

Run:

```bash
test ! -e skills/using-git-worktrees/SKILL.md
```

Expected: both commands exit successfully.

### Task 4: Verify the Complete Change

**Files:**
- Verify: `skills/`
- Verify: `tests/`
- Verify: `README.md`
- Verify: `docs/testing.md`
- Verify: `docs/superpowers/specs/2026-08-07-remove-worktree-and-branch-finishing-skills-design.md`
- Verify: `docs/superpowers/plans/2026-08-07-remove-worktree-and-branch-finishing-skills.md`

**Interfaces:**
- Consumes: all changes from Tasks 1-3.
- Produces: evidence that active runtime guidance and tests are internally consistent.

- [ ] **Step 1: Search active content for dangling skill names**

Run:

```bash
rg -n 'using-git-worktrees|finishing-a-development-branch' \
  skills tests README.md docs/testing.md \
  --glob '!skills/using-git-worktrees/**' \
  --glob '!skills/finishing-a-development-branch/**'
```

Expected: no matches. References in `RELEASE-NOTES.md` and historical files under `docs/superpowers/specs/` or `docs/superpowers/plans/` are intentionally outside this check.

- [ ] **Step 2: Run non-LLM SDD workspace tests**

Run:

```bash
bash tests/claude-code/test-sdd-workspace.sh
```

Expected: all assertions pass.

- [ ] **Step 3: Run OpenCode plugin loading tests**

Run:

```bash
bash tests/opencode/test-plugin-loading.sh
```

Expected: plugin registration, remaining skill discovery, bootstrap loading, and syntax checks pass.

- [ ] **Step 4: Review the final diff**

Run:

```bash
git diff --check
```

Expected: no whitespace errors; only the approved skill deletion, caller/test/documentation cleanup, design, and plan files are present, plus the previously approved CodeGraph initialization change if it remains in the worktree.
