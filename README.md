# Shared agent skills

The single list of skills shared by every harness (Claude Code, Codex, Pi, OpenCode, Gemini CLI, cursor-agent).
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

## Our own skills

- `to-tickets` and `implement-tickets` are ours, detached from `mattpocock/skills` on 2026-10-06:
  removed from `~/.local/state/skills/.skill-lock.json` so `npx skills update` won't overwrite them.
  They replace upstream `to-tickets` and `implement-spec`; upstream `ask-matt` still mentions
  `/implement-spec`, read that as `/implement-tickets`.
