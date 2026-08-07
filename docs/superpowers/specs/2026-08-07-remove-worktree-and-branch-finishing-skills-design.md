# Remove Worktree and Branch Finishing Skills Design

## Goal

Remove `using-git-worktrees` and `finishing-a-development-branch` from the
bundled skill set without leaving active workflows that try to invoke missing
skills.

## Behavior

Plan execution runs in the current working directory. Superpowers no longer
creates, verifies, owns, or cleans up Git worktrees.

After an implementation plan is complete, the execution workflow runs its
required verification and review steps, then reports the resulting status to
the human partner. It does not automatically merge branches, create pull
requests, delete branches, or clean up worktrees.

## Changes

- Delete `skills/using-git-worktrees/` and all supporting files in that skill.
- Delete `skills/finishing-a-development-branch/` and all supporting files in
  that skill.
- Remove worktree setup and branch-finishing requirements from
  `executing-plans`.
- Update `subagent-driven-development` setup, flowchart, final-review handling,
  finish instructions, and examples to use the current directory and report
  completion directly.
- Remove the worktree execution context from `writing-plans`.
- Remove both skills from the current workflow and skill inventory in
  `README.md`.
- Remove tests that validate behavior belonging exclusively to the deleted
  skills and update active test documentation and runners accordingly.
- Remove active references that would cause an agent to invoke either deleted
  skill.

## Historical Content

Historical release notes and completed design or implementation documents
remain unchanged. Their references describe behavior that existed at the time
and are not runtime instructions.

## Verification

- Search active skills, runtime code, current README content, and active tests
  for references to either deleted skill.
- Confirm both skill directories are absent.
- Run the remaining relevant shell-based skill tests that do not require a
  frontend build or development server.
