---
name: Always ask before push and finalize
description: After committing and advancing bookmarks, stop and ask the user before pushing or finalizing — never auto-push
type: feedback
---

After committing both repos and advancing bookmarks, STOP and
present the push/finalize commands for user approval. Do not
execute push or finalize without explicit user confirmation.

**Why:** CLAUDE.md step 6 says "Only push if the user asked to push"
and step 5 says "Ask for approval to push and finalize." The user
was frustrated when I auto-pushed and finalized after they only
approved the commit.

**How to apply:** The commit approval ("y") covers only steps 3-5
(commit + bookmarks). Always pause after bookmarks and ask
"OK to push and finalize?" as a separate approval gate.
