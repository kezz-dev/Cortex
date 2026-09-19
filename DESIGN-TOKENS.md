# Design Tokens

The visual system Cortex actually ships with, plus the reasoning behind the
decisions that got us here — so a future session doesn't relitigate them
from zero.

## Palette

| Token | Value | Role |
|---|---|---|
| `--navy` | `#0a1629` | Base anchor — background in dark mode, primary text/UI color in light mode |
| `--navy2` | `#111e35` | Slightly lighter navy — cards/surfaces sitting on top of the base |
| `--gold` | `#3b8f8a` (teal) | Brand accent — buttons, active states, the logo mark. *Named `--gold` in code for historical reasons (it held the old gold hex before the rebrand) — the variable name is stale, the value is teal.* |
| `--gold2` | `#2d726e` | Darker teal — hover/pressed states, gradients |
| `--cream` | `#f8f6f1` | Light-mode background / dark-mode primary text |
| `--green` | `#22c55e` | **Semantic only** — correct-answer states. Never used for branding. |
| `--rose` | `#f43f5e` | **Semantic only** — wrong-answer / error states. Never used for branding. |
| `--amber` | `#92400e` (+ `--amber-bg`) | **Semantic only** — warning/caution badges, distinct from any brand color |

## Why teal, not gold, not red

Three real decisions got made and reversed here — worth keeping the reasoning:

1. **Gold → dropped as "too close to the old identity."** The original
   Post-UTME app used navy/gold/cream. Reusing gold as-is for Cortex would
   have meant a reskin, not a remodel — the accent needed to change to
   signal an actual break from the parent app.
2. **Red → ruled out.** The app already uses `--rose` (a red) exclusively
   for wrong-answer and error states. Making the *brand* accent red too
   would have put "you got this wrong" and "this button is important" in
   the same color family — actively bad for a CBT app where that
   distinction has to be instant.
3. **Amber/copper vs. teal → teal won**, specifically because it reads as
   more different from the original app than amber/copper does (which is
   gold's close cousin). Teal also shares no hue-family overlap with
   green, rose, or amber, so there's no ambiguity risk as the UI grows.

## Typography

- **Display / headings:** `'Lora', serif` — used for h1s, screen titles,
  anything that should feel like a printed exam paper rather than a generic
  app
- **Body / UI chrome:** `'Inter', system-ui, sans-serif` — labels, buttons,
  nav, anything functional
- Both are carried over unchanged from the original app; no typography
  decisions were revisited during the Cortex remodel

## Logo mark

A flattened "C" — solid teal ring on navy, no gradient, no glow. This was a
deliberate choice over a first-pass AI-generated version that used a glossy
gradient + particle-scatter effect: that version read as generic "AI app
icon" style and didn't match the flatter, more editorial feel of the
navy/teal/cream system. The flattened version is what ships in
`manifest.json`'s icons.

## Component patterns worth knowing

- **Pill-style icon buttons** (`.mode-toggle`) — small circular buttons top
  of the hero (dark mode toggle, Library Manager entry). Any new global
  action that belongs on the home screen should probably follow this same
  pattern rather than introducing a new button style.
- **Cards** (`.t-panel`, `.stat-card`) — flat, `--navy2` background, thin
  border, no heavy shadows. Keep new UI consistent with this rather than
  reaching for elevation/shadow-heavy card styles.
- **Correct/incorrect states** always use `--green` / `--rose` respectively,
  everywhere in the app, with no exceptions — this consistency is what
  makes the "ruled out red as a brand color" decision above matter in the
  first place.

## What's deliberately *not* themed yet

The Format Workshop and Stock Mechanism (see `ISSUES.md`) don't have their
own visual language decided beyond what's in the interactive design
preview built for the Workshop — that preview used a "highlighter pen"
metaphor (each tag gets its own marker-cap color: teal/amber/green/purple/
steel-blue) that hasn't been reconciled with this token table yet. Treat
that as an open decision, not a finished spec.
