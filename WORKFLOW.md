# Cursor Review Rules — Workflow

A quick guide on getting useful code reviews out of Cursor instead of the usual "this looks good!" non-feedback.

## When to use which preset

| Preset | Use when |
|---|---|
| `security.cursorrules` | The PR touches auth, input parsing, file paths, subprocess calls, network egress, dependencies, or anything user-facing |
| `performance.cursorrules` | The PR touches a request handler, query, loop over collections, or anything that runs on every request |
| `tests.cursorrules` | Always — but especially when you suspect the author wrote tests after the implementation |
| `architecture.cursorrules` | The PR is medium-large (>5 files), introduces a new module, or restructures an existing one |

You don't run all four every time. Pick one or two based on the PR's risk shape.

## Two-pass workflow

**Pass 1 — broad scan.** Drop in the most relevant preset, open the diff in Cursor, and ask: "Review this PR." Read the verdict line first; that's the headline. Read the top 3 findings; decide ship / fix / hold.

**Pass 2 — deep on findings (only if needed).** For each finding worth investigating, ask Cursor: "Show me the exact diff hunk you're flagging and walk through how the bug fires." This forces the model to ground its claim in the file content rather than pattern-matching.

## Switching presets

The simplest setup: keep four files at repo root with explicit names —

```
.cursorrules.security
.cursorrules.performance
.cursorrules.tests
.cursorrules.architecture
```

— and copy the relevant one to `.cursorrules` before each review pass:

```sh
cp .cursorrules.security .cursorrules
```

Or maintain `.cursorrules` as a symlink and `ln -sf` between them.

## Customizing for your codebase

Each preset has a list of "specifically check for" bullets. Edit them. If your codebase has a particular failure mode (e.g., "we leak data through GraphQL field resolvers"), add a bullet for it. The presets here are a sensible default, not a holy text.

The single highest-leverage edit you can make: add a `## Authorial intent` section at the top of the preset describing what the author was trying to do. Reviews improve dramatically when the model knows what the change is supposed to accomplish — it can flag deviations from intent rather than only code smell.

## Recipes I personally use

**"Is this safe to merge?"** — security preset, then ask: "Given this PR, what is the worst attacker-controlled input I should fuzz before merge?"

**"Why is this PR so big?"** — architecture preset, then ask: "Suggest a smaller, safer first PR that delivers half the value."

**"This test passed but I don't trust it."** — tests preset, then ask: "Write a failing version of this test that demonstrates a behavior the implementation does NOT guarantee."

**"Will this break in prod?"** — performance preset, then ask: "What's the failure mode at 10x current traffic?"

## Related — for non-Cursor flows

If you do code review through the terminal (Claude Code, plain Claude / GPT chat, or any tool that takes a markdown context), use **[prpack](https://github.com/Lucas2944/prpack)** — same review philosophy, different shape. prpack packs the whole PR into one markdown file you paste into the model, and there's a Pro pack of `.prpack.yml` configs that mirror these four review styles: https://scottthurman89.itch.io/prpack
