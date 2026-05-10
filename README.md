# Cursor Review Rules

Four `.cursorrules` files that turn Cursor's chat into a focused code reviewer instead of a chatty assistant. Drop one into your repo root, open the diff, and ask Cursor to review.

## What's inside

```
cursor-review-rules/
├── security.cursorrules
├── performance.cursorrules
├── tests.cursorrules
├── architecture.cursorrules
├── WORKFLOW.md           — when to use which preset
└── README.md
```

Each preset:

- Re-frames the assistant as a senior reviewer with a single concern (security / perf / tests / architecture).
- Forces a structured finding format: severity, file:line, why it matters, fix.
- Demands a one-line verdict at the end (`ship / fix-before-ship / hold`) — no hedging.
- Tells the model to skip irrelevant sections rather than padding.

## How to use

```sh
# Drop the preset you want at repo root
cp cursor-review-rules/security.cursorrules .cursorrules

# Open the diff in Cursor and chat:
> Review the changes in this PR using the rules above.
```

Switch presets by replacing `.cursorrules`. Most people keep four files around as `.cursorrules.security`, `.cursorrules.perf`, etc., and `cp` the relevant one before each review pass.

## Why a separate pack?

These rules are a port of the [prpack Pro presets](https://scottthurman89.itch.io/prpack), which target generic LLM contexts. The Cursor flavor leans into Cursor's specific in-IDE rendering and uses inline file references that Cursor already resolves.

If you're using a CLI flow with Claude / GPT / etc., grab prpack instead. If you live in Cursor, this pack is shaped for you.

## License

Yours to use, modify, and embed in your projects. Don't redistribute as a paid product of your own.

Updates are pushed to your itch.io library.
