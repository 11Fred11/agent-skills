---
name: to-tickets
description: Turn a spec or conversation into contract tickets, grouped into lanes with blocking edges, that a cheaper model can implement without guessing. Feeds /implement-tickets.
disable-model-invocation: true
---

# To Tickets

Break a spec, plan, or conversation into **contract tickets**: tracer-bullet slices whose every open decision is already made, so an implementer on a cheaper model tier executes rather than designs. This is where the strong model's thinking is spent; `/implement-tickets` spends as little as possible after it.

Where tickets live and which triage labels to use should be documented in the repo's agent docs (for example `docs/agents/issue-tracker.md` and `docs/agents/triage-labels.md`). If they aren't, ask the user where tickets live and which labels to apply, then record the answer there.

## Process

### 1. Gather context

Work from the conversation. If the user passes a reference (spec path, issue number or URL), fetch its full body and comments.

### 2. Explore the code the tickets will touch

Read enough of the codebase to name, for every ticket, the files and functions it changes, the tests that cover them, and the validation commands. Use the project's glossary vocabulary and respect its ADRs. Look for prefactoring that makes the change easy; it becomes its own ticket, first.

Done when you could write the "Where" and "Verify" sections of every ticket from what you've read.

### 3. Draft vertical slices

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer it needs (schema, API, UI, tests).
- A completed slice is verifiable on its own.
- Each slice fits one fresh context window.
- Prefactoring comes first.

</vertical-slice-rules>

**Wide refactors** (one mechanical change fanning across the codebase) are sequenced expand–contract instead: expand (new form beside old), migrate call sites in batches sized by blast radius (each blocked by the expand), contract (delete the old form, blocked by every batch).

Give each ticket its **blocking edges**: the tickets that must merge before it starts.

### 4. Make every decision

Walk each ticket and list every choice an implementer would otherwise make: which variant, which default, what happens in the empty, error and loading states, what copy appears, which seam the tests use, what is out of scope. Decide each one. A choice that is genuinely the user's goes into the quiz in step 6; nothing reaches a ticket as "decide X" or "your call".

Done when no ticket contains an open question.

### 5. Group tickets into lanes and tiers

A **lane** is a group of tickets one implementer works in order inside a single context: tickets that touch the same files or the same screen share a lane, so they never conflict in parallel and the implementer reuses what it already read. Aim for lanes of 1–4 tickets; tickets in different lanes must not edit the same files. Blocking edges may cross lanes.

Give each lane a **tier**:

- **standard** (default): fully specified work. Runs on the cheaper model tier (e.g. Sonnet) at medium effort.
- **strong**: work that stays wide or subtle even after step 4 (a theme or design-token overhaul, a cross-cutting refactor, concurrency or security-sensitive code). Runs on the strongest tier (e.g. Opus).

Reserve shared resources here so parallel lanes can't collide: migration numbers, new route paths, new token names.

### 6. Quiz the user

Present the breakdown grouped by lane. For each ticket: title, lane and tier, blocked by, what it delivers, and the decisions you made that the user might want to overturn. List the questions that are genuinely the user's.

Ask whether the granularity, lanes, edges and decisions are right. Iterate until the user approves.

### 7. Publish

Publish in dependency order (blockers first) so edges can reference real identifiers.

- **Local files**: one file per ticket under `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`.
- **A real tracker (GitHub, Linear, …)**: one issue per ticket, as sub-issues of the parent when there is one, using the native blocking relationship where it exists. Apply `ready-for-agent` unless told otherwise.

Do NOT close or modify any parent issue beyond linking sub-issues.

<ticket-template>

## What to build

The behaviour this ticket makes work, from the user's perspective, in a few sentences.

## Lane

`<lane name>` · tier `standard|strong` · order `<n>` in lane

## Decisions

The choices made in step 4, one line each: "Empty state shows X", "Fold shows the newest 4", "Unavailable step never counts as done". Every decision the implementer needs; none left open.

## Where

Files and functions to change (paths with symbols, not line numbers), the existing tests to extend, and **off-limits**: files another lane owns. Pointers are starting points; the implementer confirms them against the code.

## Tests

The seam to test at (component render, HTTP endpoint, pure function) and the cases: each acceptance criterion maps to at least one test.

## Acceptance criteria

- [ ] Checkable criterion: an observable state or a command result, never "looks good".

## Verify

The narrowest commands that prove the ticket (test files, typecheck, a grep), plus any manual check that needs a browser or real service, flagged as such.

## Blocked by

References to blocking tickets, or "None".

</ticket-template>

Keep each ticket as short as its decisions allow: the implementer pays for every line on every call.
