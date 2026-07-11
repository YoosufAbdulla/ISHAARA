# ISHAARA · އިޝާރާ

**Live Dhivehi fingerspelling recognition, in the browser.**

ISHAARA reads Maldivian Sign Language (MVSL) fingerspelling from a webcam and shows the matching Thaana letter in real time. It also has a data-collection mode so the model can be trained and improved over time. Everything runs on-device — no video ever leaves the browser.

Class 12S3 · ATOMS Expo 2026 · *Innovations for Society*

---

## Live demo

👉 **https://yoosufabdulla.github.io/ISHAARA/**

Open it in Chrome or Safari, allow camera access, and hold a handshape up to the camera.

- **Recognise** — hold a handshape until it locks, then (optionally) move-and-hold a direction to add the vowel (fili). The recognised letter lights up on the dashboard.
- **Settings** (cog icon) — toggle the output text, enable fili, or swap left/right.

The recogniser only knows the letters it has been given data for, so a fresh site will say *"no samples yet"* until a dataset is published (see below).

---

## Collecting & publishing data (owner)

The collection tools are hidden from normal visitors. To unlock them, add `?studio` to the URL:

👉 **https://yoosufabdulla.github.io/ISHAARA/?studio**  *(use Chrome on desktop)*

1. Go to the **Collect** tab.
2. Tap a letter in the grid, hold the handshape steady, and press **Capture** (records 30 samples).
3. Repeat across letters — aim for the per-letter goal shown on the progress bar.
4. Press **Publish** and enter your GitHub details + a token (see below). This commits `dataset.json` to the repo, and the live site updates for everyone in ~1 minute.

You can also **Export** the dataset as a JSON file at any time.

### GitHub token for publishing
Create a **fine-grained personal access token** scoped to *only this repo* with **Contents: Read and write**
(GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens).
The token is stored only in your browser (localStorage) — it is never committed or shown to visitors.

---

## How it works

```
Camera frame  →  MediaPipe (21 hand landmarks)  →  63-value feature vector  →  k-NN classifier  →  Thaana letter
```

- **Hand tracking:** Google MediaPipe Hands (loaded from a CDN).
- **Features:** each hand is reduced to 21 landmarks, wrist-centred and scale-normalised into a 63-number vector.
- **Recognition:** a train-free k-nearest-neighbour classifier over the collected/published samples — so newly collected letters work immediately.
- **Fili (vowels):** added by a "move-and-hold" gesture after the consonant locks.
- **Storage:** the shared model is a `dataset.json` file committed to this repo; the site fetches it on load.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app (UI + recognition + collection) |
| `dataset.json` | The published training data (created by **Publish**) |

## Deploy

Static site — hosted on **GitHub Pages** (Settings → Pages → deploy from `main` / root). No build step.

---

## Built with

- **Google MediaPipe** — hand landmark tracking
- **Python (scikit-learn)** — offline model evaluation (accuracy + confusion matrix)
- **Claude Code** — used as an AI coding assistant during development

Made with ♥ by **12S3**.
