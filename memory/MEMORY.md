# Memory

## User Preferences

- **Commit approval**: Always present the full commit commands to the user for physical review BEFORE executing them. Do not run jj commit / git commit until the user explicitly approves.
- **Versioning**: Every plan must start with a version bump per notes/README.md "Versioning during development". Get user approval on single-step vs multi-step approach before starting.
- **Pre-commit checklist**: Before committing, run: tests, clippy, fmt, install the app, then retest.

## Lessons Learned

- **Avoid duplicate commits in .claude repo**: Committing the bot session repo multiple times in one session can create duplicate-titled commits with other session commits interleaved, making squashing non-trivial. Prefer a single commit to .claude at session end.
- **ochid trailers need updating after squash**: When squashing changes the target commit in .claude, the ochid reference in the app repo must be updated to point at the correct changeID.
- **Don't modify .claude repo programmatically mid-session**: Session data updates continuously, so bot-initiated changes to .claude can create circular modifications.

## Key Files

- notes/README.md — contains versioning workflow, todo format rules
- notes/todo.md — near-term task tracking
- notes/chores-01.md — chore details and reference links
- CLAUDE.md — dual repo commit/push workflow
