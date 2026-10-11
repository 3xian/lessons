# lessons

An [Agent Skill](https://agentskills.io/specification) that turns a coding-agent session into **at most one verified, reusable lesson** for the next similar task.

- Saves concise instructions in the project's root `LESSONS.md`.
- Runs only when requested, never automatically.
- Makes no change when the session contains nothing worth saving.
- Uses no hooks, scripts, or external services.

## Quick start

### 1. Install

From your project:

```bash
npx skills add 3xian/lessons --skill lessons
```

For a local checkout, replace `3xian/lessons` with its path. Add `--global` for global installation. See the [skills CLI](https://github.com/vercel-labs/skills) for supported agents.

### 2. Make lessons available to future tasks

Add this instruction **once** to the project guidance file that your agent actually loads:

- **Codex:** `AGENTS.md`
- **Claude Code:** `CLAUDE.md`
- **Other agents:** their supported project-instructions file

```markdown
Before working on repository tasks, read LESSONS.md if it exists. Apply only lessons relevant to the current task, and check them against the current project.
```

The skill does **not** edit these guidance files. It creates `LESSONS.md` only after finding a qualifying lesson. Loading and following the instruction depends on your agent and configuration.

### 3. Capture a lesson

After a task that involved a correction, failed approach, or useful discovery, invoke:

```text
/lessons
```

An explicit `/lessons` invocation authorizes a minimal update to the project's `LESSONS.md`, subject to the agent's file permissions. You can also ask the agent to **save a lesson from this session**.

To inspect without writing, ask it to **review the session without changing files**. A review-only request returns a proposed diff, not an applied change.

If your agent does not support slash commands, use a natural-language request. You may provide session logs you are authorized to share; the skill will not invent missing history.

## What gets saved

A single short rule with a clear situation and action, grounded in observed evidence:

```markdown
- **When changing `apps/api/**`:** Run `pnpm --filter @app/api test` for focused tests.
```

*Illustrative example only; this repository does not provide that command.*

The skill checks existing project guidance and lessons, avoids duplicates, and may revise an overlapping or outdated rule when the evidence supports it. It does not save transcripts, secrets, timestamps, or speculative advice.

## What you'll see

- **Applied:** decisive evidence, how the lesson should change the next similar task, and the small `LESSONS.md` diff that was written.
- **Proposed:** the same information as a diff, without writing files.
- **No change:** a specific reason, such as insufficient evidence, no reusable lesson, or guidance that already covers the issue.

If a session shows that an existing lesson was ignored, the skill reports that rather than storing a duplicate. It does not guess whether the problem was failure to load the file or failure to follow it.

## Check that it works

Start a **new session** on a task similar to one covered by `LESSONS.md`. Check whether the agent reads the file and follows the relevant rule **before** repeating the old mistake. Saving a lesson alone does not prove it will be used.

Full instructions: [`skills/lessons/SKILL.md`](skills/lessons/SKILL.md).

## License

[MIT](LICENSE)
