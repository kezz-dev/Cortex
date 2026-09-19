# Cortex

**Upload any PDF. Get a CBT-style practice session. Your documents, your rules.**

I built Cortex by remodeling an app I already had running with real students —
a Post-UTME exam-prep tool built for the University of Ilorin. That app
worked, and worked well, but it was locked to one university, one fixed
question bank, and one rigid upload format. Cortex is what happens when I
strip that down to the actual engine underneath and open it up: any PDF,
any subject, any course — not just one school's past questions.

Because I started from something already proven, I didn't have to rebuild
this from nothing. The CBT interface, the bookmarking, the spaced-repetition
flashcards, the category management — all of that came over largely intact.
What I actually had to design from scratch were the two things that make
Cortex *Cortex* rather than just a reskinned copy of the old app.

## What makes Cortex Cortex

These two features are the actual point of this project. Everything else is
inherited, hardened behavior from the app I built it on top of.

### 🧩 Format Workshop
Most tools that "extract questions from a PDF" assume your PDF already looks
a certain way. Cortex ships with three built-in formats out of the box
(bracket-tagged, naturally numbered with an inline-marked answer, and
answer-key-at-the-end) — but no fixed set of formats covers every PDF
someone will actually upload.

So instead of asking people to reformat their documents to match what the
app expects, the Format Workshop lets you *show* Cortex what your format
looks like: paste two real example questions, tag the question, the options,
and the correct answer (subject and explanation are optional), and Cortex
works out the repeatable pattern from there. Save it, reuse it, edit it
later, share it with someone else uploading the same kind of document.

I'm building this as a text-pattern system rather than a visual/image-based
one on purpose — coordinate-based rules break the moment a layout shifts by
a few pixels, while pattern-based rules built from two tagged examples hold
up across an entire document.

*Status: designed, interactive preview built, not wired into the live app
yet — see [ISSUES.md](./ISSUES.md).*

### 📈 Stock Mechanism
A plain percentage score doesn't tell you much about momentum. The Stock
Mechanism treats your performance like a candlestick chart instead: each
study session becomes a "period" with an opening score, a closing score, a
high, and a low, based on how you did question-to-question. String a few
sessions together and you get something like:

```
🟩🟩🟥🟩🟩🟩🟥🟥🟩
```

Green candles when you're climbing, red when you're slipping — and since
every subject gets tracked as its own "stock," you can see at a glance that
your Mathematics is climbing while your Physics is falling, instead of
staring at one flattened average that hides both.

I'll be upfront about the tradeoff: this is a psychological framing device,
not a real trading mechanism, and no money or real stakes are ever attached
to it. The obvious risk is someone optimizing to keep their candles green
instead of actually learning — worth watching for once this is live, not
something I think a UI change alone fully solves.

*Status: concept fully designed, not yet built — see [ISSUES.md](./ISSUES.md).*

## What's already in here

- **CBT practice interface** — category practice, timed mode, freestyle,
  puzzle mode, and a flexible custom mock (pick your own subjects, question
  count, and time limit)
- **Three extraction formats** — bracket-tagged, natural inline-marked
  answer, and answer-key-at-the-end, with the parser trying each in turn
- **Library Manager** — upload, rename, merge, delete, and reorder subjects;
  no subject is protected or "core" anymore, they're all just your content
- **Lock-in Mode** — an entirely opt-in daily streak system. Turn it on and
  your momentum builds like a habit tracker; break the streak and your decks
  visually fade rather than getting locked away; turn it off and it wipes
  clean, on purpose, with a confirmation first
- **Bookmarks, Leitner-box flashcards, a built-in calculator** — all carried
  over from the app this was built on
- A demo question bank (505 questions across six subjects) ships as sample
  content so the app isn't empty on first open

## Getting started

Cortex is a single self-contained HTML file (`cortex.html`) — no build step,
no server required to try it.

**Quickest way to try it:** just open `cortex.html` directly in a browser.
Everything works except the offline install prompt, which browsers only
allow over `http(s)`, not `file://`.

**To get the full installable-app experience** (Chrome's native "Install"
prompt, offline support via the service worker):

```bash
# from inside this folder
npx serve .
# or
python3 -m http.server 8000
```

Then open the printed `localhost` URL. Once served this way, `manifest.json`
and `sw.js` take over and Chrome will offer to install Cortex like a native
app — the exact same files work unchanged if you deploy this folder to any
static host (GitHub Pages, Netlify, Cloudflare Pages) later.

## Repo structure

```
cortex.html              the entire app — UI, logic, and demo data in one file
manifest.json            PWA manifest (name, icons, theme colors)
sw.js                    service worker — caches the app shell for offline use
icon-192.png             app icon
icon-512.png             app icon
icon-maskable-512.png    app icon, maskable variant for adaptive icon shapes
README.md                this file
ISSUES.md                known gaps — currently the Format Workshop and Stock Mechanism
```

## A note on the demo content

The 505 built-in questions are sample/demo content so the app has something
to show on first launch. They're not meant to be Cortex's actual product —
the real idea is that you upload your own material. I haven't done a
rigorous copyright audit of that demo set, so treat it as placeholder data,
not something to rely on or redistribute as-is.
