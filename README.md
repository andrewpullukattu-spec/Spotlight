# Spotlight

**One of you is lying.** A four-phase party game of questions, bluffing, and one very bright light.
Everyone answers the same question — except one player, who is secretly answering a different one.
Talk it out, vote, and see who the spotlight catches.

Rebrand and redesign of `Odd1OwtApp`. Same Firebase backend, same game logic.

---

## Commit this into the new repo

```bash
# create the repo on github.com/new as "spotlight" (empty, no README), then:
cd spotlight
git init
git add .
git commit -m "Spotlight: rebrand from Odd1Owt, splash screen, spotlight reveal"
git branch -M main
git remote add origin https://github.com/andrewpullukattu-spec/spotlight.git
git push -u origin main
```

Or with the GitHub CLI, which creates the repo for you:

```bash
cd spotlight
git init && git add . && git commit -m "Initial commit"
gh repo create spotlight --public --source=. --push
```

---

## What changed from Odd1OwtApp

**Brand**
- Odd1Owt → **Spotlight** across all pages, titles, and the manifest.
- The hidden role is now **the Imposter** (was "the Odd1Owt"). Verdicts stay CAUGHT / ESCAPED.
- Accent moved from alarm red `#e03030` to spotlight amber `#e08b1f`. `--red` is kept as an alias
  so any rule still referencing it resolves — but new work should use `--hot`.
- Background lifted off pure black to `#0c0d10`; cards to `#15171c`.

**New: splash screen** (`#splash` in `index.html`, styles at the bottom of `style.css`)
- Beam sweeps in from off-axis, the wordmark's **O** ignites, "ONE OF YOU IS LYING" lands. ~1.7s.
- Pure CSS, so it plays *while* the Firebase module loads rather than after it.
- `game.js` removes it on init, with a 1.7s floor so the animation never gets cut mid-sweep.
- Honours `prefers-reduced-motion`.

**New: spotlight reveal** (Phase 4)
- Step 1 gets a `.search-beam` that hunts across the screen during the countdown.
- Step 2 gets a `.landed-beam` — the light has found someone.
- Step 3 recolours the beam behind the verdict banner: green on CAUGHT, red on ESCAPED.

**Icons**
- `icon.png` (1024), `icon-512.png`, `icon-192.png` — the amber tile with the wordmark's `S`.
- `title.png` — the wordmark with the lit `O`.
- ⚠️ These were generated with a fallback weight, not Archivo Black. Re-export from the
  design file before you ship to the stores.

**Deliberately unchanged**
- `game.js` game logic, phase machine, and scoring (+1 to all non-Imposters on CAUGHT,
  +1 to the Imposter on ESCAPED).
- The Firebase config — still points at the `odd1owt-ed87d` project. Your data and lobbies
  are intact. Rename the Firebase project separately if you want, and update `game.js` lines 7–14.
- `localStorage` keys (`odd1owt_pid`, `odd1owt_name`, `odd1owt_unlockedDecks`). Renaming these
  would silently log every existing player out of their saved name. Left alone on purpose.
- `questions.js` — decks and questions are byte-identical.

---

## Still to do

- Real icon export at store resolutions with the correct typeface.
- Optimistic UI on vote taps: paint the selection immediately, reconcile with Firestore after.
  Currently every vote waits on a network round-trip, which is the one place the app feels slower
  than the prototype.
- "Waiting on <name>…" in the phase status line, so a slow connection reads as someone thinking
  rather than the app hanging.
- A series-end winner screen. Right now the game loops rounds with no terminus.

## Files

```
index.html       home, lobby, 4 phase screens, splash
style.css        full app styling + splash + spotlight beams
game.js          Firebase wiring, phase machine, scoring  (unchanged except reveal copy + splash removal)
questions.js     deck definitions                          (unchanged)
manifest.json    PWA manifest
about.html  contact.html  how-to-play.html  privacy.html
icon.png  icon-512.png  icon-192.png  title.png
ads.txt
```
