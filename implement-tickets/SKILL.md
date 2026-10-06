---
name: implement-tickets
description: Implement the contract tickets from /to-tickets on one integration branch, one implementer per lane, with a thin coordinator.
disable-model-invocation: true
---

You have been given contract tickets from `/to-tickets` (or a spec pointing at them). Goal: every ticket implemented and verified on one **integration branch**, each resolved the way the issue tracker closes work, for the fewest total tokens including rework.

Where tickets live and which triage labels to use should be documented in the repo's agent docs (for example `docs/agents/issue-tracker.md` and `docs/agents/triage-labels.md`). If they aren't, ask the user where tickets live and which labels to apply, then record the answer there.

You are a **thin coordinator**. The thinking happened in `/to-tickets`; your job is dispatch, merge, verify and close. Keep your own context small, because every turn re-reads it:

- Hold pointers (ticket IDs, commit SHAs, file paths), not copies of ticket bodies, diffs or logs.
- Read only implementers' final reports, never their transcripts.
- Keep screenshots out of your context unless a ticket needs a visual judgement; then view viewport-sized captures, one at a time.
- At each phase boundary (after a wave of merges, before review), suggest the user run `/compact` when your context is large.

## Implementer contract

Every implementer works under this contract. Put it in the shared notes (step 2) and point each implementer at it:

- **Lane:** work the lane's tickets in order in one worktree on one branch, a commit (or a few) per ticket, each message prefixed with the ticket ID.
- **Build:** call the Skill tool with `tdd` for each ticket. The ticket names the seam and the cases; use them without asking.
- **Decisions:** follow the ticket's Decisions. If the code contradicts a ticket, or a needed decision is missing, stop that ticket and report the gap rather than guessing.
- **Silent:** no narration between tool calls. Run quiet commands (dot or summary test reporters, no verbose flags, `-q` git), read file ranges rather than whole large files, and print no diffs to check your own work. Quiet on success; full output on failure.
- **Validate once:** run the targeted tests while working, and the ticket's Verify commands once, after merging the integration branch tip into your branch, just before reporting.
- **Report** in at most 150 words: branch, commits per ticket, Verify results (failures in full), any gap or deviation, and anything left undone.

## Steps

1. **Read the tickets** and build the task graph: lanes, tiers, blocking edges. Check each ticket against the contract shape (Decisions, Where, Tests, Acceptance, Verify). A ticket with an open question or no Verify goes back to the user to fix via `/to-tickets` before any work starts; implementers don't fill design gaps.

2. **Write the shared notes** in one file outside the repo: the implementer contract above, the integration branch name, setup commands for a fresh worktree, reserved shared resources (from the tickets), project rules implementers must follow, and known pre-existing test failures. Point to repo docs rather than copying them.

3. **Create the integration branch.** If the tracker closes work through PRs, or the user asks for one, open a draft PR after the first merge, marked as closing the tickets.

4. **Dispatch the lane frontier**: lanes whose first ticket's blockers are all merged. Run **at most 3 implementers at once**, each in its own worktree branched from the integration branch, in the background. Pick the model by the lane's tier: standard runs on the cheaper tier (e.g. Sonnet) at medium effort; strong runs on the strongest tier (e.g. Opus). The dispatch prompt is pointers only: the notes file, the lane's ticket IDs, the branch name. Mark the lane's tickets in progress.

5. **Merge inline** as each implementer reports: yourself, not through another subagent. Check the branch contains the integration tip, `git merge --no-ff` with a message naming the tickets, then run the lane's Verify commands on the integration branch. On a conflict or a red Verify, resolve small breaks yourself; send larger ones back to the same implementer with the failure output.

6. **Refill the frontier** when merges unblock lanes, keeping at most 3 running. If a usage limit interrupts implementers, resume them one at a time once it resets, each with a one-line pointer to where it stopped.

7. **Review once**, after every lane has merged: call the Skill tool with `code-review` on the integration branch against the base. Build the fix list from defects and spec gaps only. Collect standards and smell findings into one backlog ticket unless a fix is trivial and touches the same lines. Hand the fix list to a single implementer under the same contract, on the cheaper tier unless a fix needs the strongest, and merge it inline.

8. **Verify end to end once**, on the final integration branch, with the project's own run or verify workflow: exercise each ticket's manual Verify checks. Capture evidence a ticket's acceptance asks for (screenshots, command output) to files, and attach it to that ticket when the tracker supports attachments. A failure goes back through step 7's fixer.

9. **Close out**: resolve every ticket the way the tracker closes work, with a short comment (what shipped, how it was verified, anything left open). Mark a draft PR ready if one exists, otherwise report the integration branch. Remove the implementer worktrees.

New requirements that arrive mid-run become new tickets through `/to-tickets`, ideally in a fresh session, rather than being appended to this run.
