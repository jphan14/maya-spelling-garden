# Maya's Spelling Garden

Live at: **https://jphan14.github.io/maya-spelling-garden/**

This repo is fully self-contained — `words.json`, `scripts/generate_audio.py`,
and the `audio/` cache all live here, so the whole update workflow works
from any fresh clone of this repo. You don't need the original Windows
project folder (`../Games/maya-spelling-garden.html` is kept as a local
mirror only, for convenience browsing on that machine — it's not required).

## Adding a new week (from a computer)

```bash
# 1. Add the new week to words.json (see format below), then:
python scripts/generate_audio.py
#    - generates mp3s only for words that don't already have cached audio
#    - re-embeds all audio as base64 into index.html

# 2. Commit and push — GitHub Pages redeploys automatically within ~30s
git add -A
git commit -m "Add <week label>"
git push
```

`words.json` format — append a new object to the `weeks` array. This repo
tracks the "First Grade Journeys" spelling curriculum (30 lessons, 6
units, one lesson assigned per week), so `id`/`label` follow "lessonN":
```json
{
  "id": "lesson31",
  "label": "Lesson 31",
  "unit": 7,
  "weekOf": "2027-03-29",
  "focus": "Short Vowel o",
  "words": [
    { "word": "hop" },
    { "word": "dog", "emoji": "🐶" }
  ]
}
```
- `"unit"` groups lessons under a colored header in the vertical lesson
  list (colors cycle every 6 units — see `UNIT_COLORS` in index.html).
- `"weekOf"` is the Monday the lesson's week starts (`YYYY-MM-DD`). The
  home screen automatically highlights whichever lesson's Mon–Sun range
  contains today's date as "Today's Lesson" — no manual step. If a lesson
  ever gets skipped or the school's pace shifts, just adjust that lesson's
  (and any later lessons') `weekOf` dates to match reality.
- Only add `"emoji"` for words with one clear, unambiguous picture — skip
  it for sight/function words (am, is, at, etc.).

## Adding a new week from your phone

This is the workflow for "I have a new word sheet from school, I'm not at
my computer":

1. Take a photo of the word sheet.
2. Open a Claude Code session against this repo — either the claude.ai/code
   mobile web app, or the Claude mobile app if it supports opening a Code
   session, pointed at `jphan14/maya-spelling-garden` (have it clone the
   repo if it isn't already checked out).
3. Send the photo in the chat and ask Claude to add those words as a new
   week. Claude can read the words directly off the photo, edit
   `words.json`, run `scripts/generate_audio.py`, and push — all inside
   that one session, no desktop needed.
4. GitHub Pages rebuilds automatically within ~30 seconds; refresh the
   live URL on Maya's iPad to see the new week.

If a mobile Code session isn't available when you need it, the fallback is
editing `words.json` by hand in the GitHub mobile app or website (Edit
file → commit directly to main) and asking Claude to run step 1's script
and push the next time you're at a computer — the audio just won't exist
for those words until that happens (the app falls back to the device's
built-in text-to-speech for any word missing embedded audio, so it still
works in the meantime, just with lower-quality speech).

## Voices

The app embeds one voice (Jenny) that plays automatically on word load and
via the "hear it again" button. `scripts/generate_audio.py`'s `VOICES` list
also has Andrew and Emma defined but commented out — the full 240-word
curriculum at 3 voices pushed the page to 11.8MB, so it's Jenny-only for
now (~3.5MB). Andrew/Emma's audio is already cached in `audio/`, so
uncommenting them and rerunning the script (e.g. for a shorter custom word
list) won't need new TTS calls.

## Requirements

`pip install edge-tts` — needed wherever `scripts/generate_audio.py` runs
(your computer, or the cloud sandbox behind a Claude Code session).
