# Deploy folder — do not hand-edit

This is a clone of https://github.com/jphan14/maya-spelling-garden, the
GitHub Pages repo that serves the live game at:

https://jphan14.github.io/maya-spelling-garden/

`index.html` here is just a copy of `../Games/maya-spelling-garden.html`.
The real source of truth is `../Games/maya-spelling-garden.html` —
always edit that file (or `../words.json` + `../scripts/generate_audio.py`
for word/audio changes), never this copy directly.

## To publish an update (e.g. after adding a new week)

```bash
# 1. Add the new week to words.json, then regenerate audio + re-embed it
python ../scripts/generate_audio.py

# 2. Copy the updated file into this deploy folder as index.html
cp ../Games/maya-spelling-garden.html index.html

# 3. Commit and push — GitHub Pages redeploys automatically within ~30s
git add index.html
git commit -m "Update words"
git push
```
