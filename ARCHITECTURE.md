# Architecture

Cortex is one self-contained HTML file (`cortex.html`) — no build step, no
modules, no bundler. Everything lives in a single `<script>` block. This
doc is a map for finding your way back into that file quickly, not a
line-by-line spec.

## Why single-file

This carries over from the app Cortex was remodeled from. The whole point
is zero-friction distribution: open the file and it works, no server, no
install step, no dependency resolution. The tradeoff is that navigating the
file cold is harder than a multi-file project would be — hence this doc.

## Load order, roughly top to bottom

1. **`<head>`** — meta tags, manifest link, inline `<style>` block (all CSS,
   using CSS custom properties as design tokens — see `DESIGN-TOKENS.md`)
2. **`<body>`** — every screen's HTML sits in the DOM at once, each wrapped
   in `<div class="screen" id="s-...">`. Navigation is just toggling an
   `active` class via `go(id)` — nothing is ever removed or re-fetched.
3. **`<script>`** — data, then state, then every function, roughly grouped
   by feature with comment-banner headers (`/* ═══ SECTION ═══ */`)

## Finding things fast

| Looking for... | Search for... |
|---|---|
| The demo question bank | `BASE_Q`, `ALL_Q`, `adminBank` |
| Storage schema / defaults | `let storage = {` |
| Loading/saving persisted state | `loadStorage()`, `saveStorage()` |
| Screen navigation | `function go(id)` |
| Extraction — dispatcher | `function detectQuestions(` |
| Extraction — bracket-tag format | `detectQuestionsBracketFormat` |
| Extraction — natural inline-marker format | `detectQuestionsNaturalFormat` |
| Extraction — answer-key format | `detectQuestionsAnswerKeyFormat` |
| Document title guessing (PDF/text) | `guessPDFTitle`, `guessTitleFromPlainText` |
| Upload pipeline (file → questions → bank) | `runPipeline`, `pipeStep` |
| Quiz session engine | `session` object, `startMode()`, `renderQ()`, `finishSession()` |
| Lock-in Mode (streak/momentum/decay) | `updateMomentum()`, `handleLockInToggle()`, `storage.decayed` |
| Library Manager (rename/delete/merge categories) | `enterAdmin()`, functions prefixed `cm` (e.g. `onCmDeleteSelect`) |
| Bookmarks | `renderBookmarks()`, `storage.bookmarks` |
| Leitner-box flashcards | `puzzleBoxes` |
| Dark mode | `toggleDark()` |
| Back-button guard (prevents accidental exits) | `backGuardScreen`, the History API listener near init |
| Service worker registration | Near the bottom of init, guarded by `location.protocol` |

## Storage model

Everything persists to a single `localStorage` key via `storage` (a plain
object) and `loadStorage()` / `saveStorage()`. There's no database — the
whole app's state for one user, on one device, is this one JSON blob.
`Set` objects (`seen`, `weakIds`) get special serialization handling since
`JSON.stringify` can't handle them natively.

Two separate reset paths exist and matter differently:
- **Reset Progress** — clears scores/streaks/session history, but keeps
  bookmarks, category order, and Lock-in Mode's own settings.
- **Lock-in Mode disable** — a full wipe, including Lock-in Mode's own
  onboarding state, since turning it off is meant to be a genuine restart.

## Extraction pipeline shape

```
file → readXXXFile() → raw text → detectQuestions() dispatcher
                                        │
                    ┌───────────────────┼───────────────────┐
                    ▼                   ▼                   ▼
          bracket-tag format   inline-marker format   answer-key format
          ([QUESTION] etc.)    (numbered + lettered,   (numbered + lettered,
                                answer marked inline)   answer key at the end)
                    │                   │                   │
                    └───────────────────┴───────────────────┘
                                        ▼
                              array of {q, opts, ans, exp, category}
                                        ▼
                                  adminBank / ALL_Q
```

The dispatcher tries each format in order and uses whichever one actually
returns results — this is a placeholder for real auto-detect (confidence
scoring, ambiguity handling), not the finished version of it.

## What's not built yet

See `ISSUES.md` — the Format Workshop (a fourth, user-defined extraction
format) and the Stock Mechanism (candlestick-style performance charting)
are both designed but not implemented. Neither has a code location yet
because neither has code yet.
