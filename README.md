# Shared agent skills

The single list of skills shared by every harness (Claude Code, Codex, Pi, OpenCode, Gemini CLI, cursor-agent).
The workflow is adapted from [Matt Pocock's skills](https://github.com/mattpocock/skills), with our own
planning and implementation skills tuned for correct work at a lower token cost.

`/name` skills are typed by you. Skills marked *(auto)* are also reached by the agent on its own.

## The main flow: idea → ship

1. **Sharpen the idea.** `/grill-with-docs` in a repo (records decisions as ADRs and glossary entries),
   or `/grill-me` anywhere else.
2. **Detour when talk can't settle it.**
   - A question needs a runnable answer (state, logic, a UI you must see) → `/prototype`,
     bridged with `/handoff` out and back.
   - You suspect a better answer than the obvious one, or something polished could be bolder → `/unchained`.
3. **Plan it.** Small enough for this session → `/implement` here. Bigger →
   `/to-spec`, then `/to-tickets` (the strong model makes every decision and groups tickets into lanes).
4. **Build it.**
   - `/implement-tickets` runs the whole ticket set: one implementer per lane on a cheaper model,
     at most 3 in parallel, one review at the end.
   - `/implement` works one ticket per session instead, clearing context between tickets.
   - Both build with `tdd` and review with `code-review`; `pr` shapes the pull request body.

Keep steps 1–3 in one context window so the tickets carry the same thinking; compact at a phase boundary
if the session grows past ~150k tokens. Start a fresh session for building, and for new requirements
that arrive mid-build (back through `/to-tickets`).

### Main-flow skills

| Skill | What it does |
| --- | --- |
| `/grill-with-docs` | Relentless interview that sharpens a plan and writes ADRs and glossary entries as it goes. |
| `/grill-me` | The same interview without a repo to write docs into. |
| `prototype` *(auto)* | Throwaway code that answers one design question (state, logic, or a UI to look at). |
| `/handoff` | Compacts the conversation into a document another session can pick up. |
| `/unchained` | Reimagines a solution from its one key goal, builds the bolder version, and argues it against what exists. |
| `/to-spec` | Turns the conversation into a spec on the issue tracker, no interview. |
| `/to-tickets` | Splits a spec into decided contract tickets, grouped into lanes and model tiers, with blocking edges. |
| `/implement-tickets` | Implements all tickets on one integration branch with lane implementers and a thin coordinator. |
| `/implement` | Implements one piece of work from a spec or ticket in the current session. |
| `tdd` *(auto)* | Builds behaviour test-first, one red-green slice at a time. |
| `code-review` *(auto)* | Reviews a diff on two axes: repo standards and the originating spec. |
| `pr` *(auto)* | Writes a PR body with a visual, before/after evidence, and a one-way or two-way door call. |

## On-ramps

Situations that generate work, then join the main flow.

| Skill | When |
| --- | --- |
| `/triage` | Bugs and requests you didn't write are piling up; turns them into agent-ready issues. |
| `diagnosing-bugs` *(auto)* | Something hard is broken or slow; builds a tight red loop first, then fixes with a regression test. |
| `/wayfinder` | An effort too big and foggy for one session; charts decision tickets until the path is clear, then hands to `/to-spec`. |

## Situational skills

Reach for these when the moment calls for them; they sit outside the main flow.

**Thinking and communication**

| Skill | What it does |
| --- | --- |
| `grilling` *(auto)* | The interview primitive behind both grill skills. |
| `/to-questionnaire` | Turns a decision you can't answer alone into a questionnaire for someone else. |
| `/wait-what` | Your last message didn't land: re-pitches it plainly. |
| `/teach` | Teaches you a skill or concept inside the current workspace. |

**Codebase health**

| Skill | What it does |
| --- | --- |
| `/improve-codebase-architecture` | Surveys the code for deepening opportunities in an HTML report, then grills the one you pick. |
| `codebase-design` *(auto)* | Shared vocabulary for designing deep modules, interfaces and seams. |
| `domain-modeling` *(auto)* | Builds the project's domain model: glossary terms and ADRs. |

**Research, knowledge and tooling**

| Skill | What it does |
| --- | --- |
| `research` *(auto)* | Investigates a question against primary sources and saves the findings to the repo. |
| `librarian` *(auto)* | Recalls from, and files into, the owner's personal knowledge wiki. |
| `find-skills` *(auto)* | Finds and installs skills for a task you describe. |
| `writing-for-agents` *(auto)* | How to write skills, `AGENTS.md` and other documents agents read. |
| `wizard` *(auto)* | Generates an interactive bash wizard for steps only a human can perform. |

**System**

| Skill | What it does |
| --- | --- |
| `diagnose-crash` *(auto)* | Explains a local program crash from its systemd core dump. |
| `omarchy` *(auto)* | Customises the Omarchy desktop: Hyprland, themes, bar, terminals. |

**Setup and routing**

| Skill | What it does |
| --- | --- |
| `/setup-matt-pocock-skills` | One-time repo setup for the engineering skills: issue tracker, triage labels, domain docs. |
| `/ask-matt` | Asks which skill fits your situation (upstream router; read its `/implement-spec` as `/implement-tickets`). |

## How the folder is wired

A skill that belongs to one harness only stays in that harness's own folder, not here.

- Pi, OpenCode, Gemini CLI, Codex and cursor-agent read this folder directly.
- Claude Code only reads `~/.claude/skills`; `.tools/link-claude-skills` links each skill here into it
  and runs on every Claude Code session start (SessionStart hook in `~/.claude/settings.json`).
- OpenCode also reads `~/.claude/skills`, which would leak Claude-only skills into it;
  `OPENCODE_DISABLE_CLAUDE_CODE_SKILLS=1` in `~/.bashrc` turns that off.

## Adding and updating

- Add from a repo: `npx skills add <owner/repo> -g -s <skill>`, then pick only universal agents.
- `npx skills update -g` refreshes installed skills only; it never adds skills new upstream.
  List a repo's skills with `npx skills add <owner/repo> -l`.
- Updates overwrite local edits to third-party skills. Review with `git diff` after updating
  and keep or re-apply your refinements before committing.
- Our own skills (`to-tickets`, `implement-tickets`, `unchained`) aren't tracked by `npx skills`:
  `to-tickets` and `implement-tickets` were detached from `mattpocock/skills` on 2026-10-06 by removing them
  from `~/.local/state/skills/.skill-lock.json`, so updates leave them alone.
