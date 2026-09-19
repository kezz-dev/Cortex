# Known Issues

Cortex is a remodel of a working, already-tested app (the original Post-UTME
practice app), so most of what's shipped here carries over proven behavior —
open issues are minimal. The two real gaps are Cortex's own specialty
features, both still in design/prototype stage rather than built into the
live app yet.

## Open

### 1. Format Workshop not yet implemented
The tool for teaching Cortex a new question format by tagging two example
questions (question / options / correct answer, with subject and explanation
optional) is designed and has an interactive design preview, but isn't wired
into the real app yet. Right now, only three built-in formats are supported:
bracket-tagged, natural inline-marked, and answer-key-at-the-end.

### 2. Stock Mechanism not yet implemented
The candlestick-style performance visualization — treating a study session's
score movement like a stock chart (open/close/high/low per session, green/red
"candles," per-subject "stocks" you can compare) — is a specialty feature
planned for Cortex but not yet built. This idea was originally explored for
the Post-UTME app but never shipped there either.

## Notes
Both of the above are Cortex's actual point of difference from the app it
was remodeled from — everything else is largely carried-over, hardened
behavior. Treat these two as the priority build items going forward.
