---
name: timed module design decisions
description: Design rationale for src/timed.rs — Timed struct and timed! macro with single/dual parameter forms
type: project
---

## timed! macro has two forms for different use cases

**Single parameter** — `timed!("label")`: Creates a drop guard. Prints elapsed
time to stderr when the variable drops at end of enclosing scope. Use for timing
whole functions or arbitrary scopes (including loop bodies and `{ }` blocks).
No visible `let` binding in user code — the macro hides `let _timer = ...`.

**Dual parameter** — `timed!("label", { ... })`: Wraps a code block. Prints
elapsed time to stderr immediately after the body runs via `.eprintln()`.
Variables declared inside the body escape into the calling scope because the
`{ }` is a macro token delimiter, not a Rust block — the body expands as bare
statements.

**Why:** Two forms needed because:
- You can't wrap an entire `fn` body in the block form without indenting everything
- The block form can't be used as a function-level attribute (it expands to statements, not items)
- The single-param form avoids exposing a `let mut _timer` that looks unused

**How to apply:** Use single-param at top of functions, dual-param for inline sections.

## Key design decisions

- **stderr only** — timing output goes to stderr so stdout program output stays
  clean and pipeable. User confirmed this after initially preferring println.
- **`silent` flag** — `.get()` and `.eprintln()` set `silent = true` so `Drop`
  doesn't double-print. `Drop` calls `self.eprintln()` only if not silent.
- **DRY** — `Drop` calls `eprintln()` which calls `get()`. Single print path.
- **`cargo fmt` fights indentation** — fmt always expands macro bodies to
  double-indented multi-line. Single-line `timed!("x", { let y = foo(); });`
  gets expanded. Outdenting doesn't survive either. Accept fmt's formatting.
- **Replaced `TimeIt`** — `Timed` subsumes `TimeIt` (which had no `.get()`
  or `.eprintln()`). Single struct for all timing needs.
