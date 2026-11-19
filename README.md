# Oops/Wrong Layout

<img src="icons/icon-512.png" alt="" width="96" align="right">

[![Live site](https://img.shields.io/badge/live%20site-GitHub%20Pages-2ea44f?logo=github)](https://unknown-x0.github.io/Oops-Wrong-Layout/)
[![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

Two small, dependency-free browser tools for a keyboard gone wrong: fix text typed under the wrong keyboard layout, or turn Japanese romaji into kana. Everything runs in your browser.

**[Open Oops/Wrong Layout →](https://unknown-x0.github.io/Oops-Wrong-Layout/)**

![Oops/Wrong Layout](assets/preview.png)

## Features

- **Layout Fix (`index.html`):** undoes text typed in the wrong keyboard layout, across eight layouts.
- **Romaji → Kana (`romaji.html`):** converts typed Japanese romaji into hiragana or katakana.
- **The mechanism on screen:** Layout Fix shows a live key map, each key carrying the character you typed (top-left) and the one you meant (bottom-right), with the keys your text used lit up; Romaji → Kana lights the kana in your result on a kana chart.
- **Typed out, like a typewriter:** pasted text types itself into the result while the matching keys strike; Copy confirms with a check and a small "Copied" slip.
- **Display settings:** Mode (System, Light, Dark) and Paper (Parchment, Carbon Copy, Ledger Green, Blueprint, Kraft), each paper in light and dark, remembered on your device.
- **Made for every screen:** on phones the keyboard splits into two half-boards; everything works by keyboard and with reduced motion.

## Getting started

### Use it online

Open the [live site](https://unknown-x0.github.io/Oops-Wrong-Layout/).

### Run it yourself

Open `index.html` (Layout Fix) or `romaji.html` (Romaji → Kana) directly in a browser. Both are static pages: no npm install, no bundler, no framework.

```sh
git clone https://github.com/UNKNOWN-X0/Oops-Wrong-Layout.git
open Oops-Wrong-Layout/index.html
```

## Usage

### Layout Fix

Typed a whole sentence in the wrong keyboard layout and only noticed at the end? Paste the garbled result, tell it which two layouts were involved, and it reconstructs what you meant to type.

**How it works:** every layout table maps _physical key positions_ to characters. Converting a piece of text just means: for each character, find the physical key that produced it in the layout you were typing in, then read off what that same key produces in the layout you meant to use. It's the same trick tools like Punto Switcher use — no dictionaries, no guessing, purely mechanical and reversible.

```text
"l;ylfu"  typed as US-QWERTY, meant as Thai   →  "สวัสดี"
"ghbdtn"  typed as US-QWERTY, meant as Russian →  "привет"
```

#### Supported layouts

English (US QWERTY) · Thai (Kedmanee) · German (QWERTZ) · Russian (ЙЦУКЕН) · Ukrainian (ЙЦУКЕН) · French (AZERTY) · Spanish (QWERTY ES) · Hebrew

#### Using it

1. Paste the garbled text into box **A**.
2. Set **Typed as** and **Should have been** to the two layouts involved. Each picker previews the layout's home-row keys (a s d f g on that layout).
3. The fix types itself out as you go, and the key map below lights up the keys your text used. Click **Auto-detect direction** if you're not sure which way round the layouts go.
4. Click **Copy**.

### Romaji → Kana

Type Japanese the way you would on any English keyboard; get proper kana back.

```text
konnichiwa  →  こんにちは
kitte       →  きって      (doubled consonant → small っ)
gen'in      →  げんいん    (apostrophe forces an explicit ん)
kyuuto      →  キュート    (katakana contracts repeated vowels into ー)
```

Handles the standard Hepburn conventions: doubled consonants → small tsu, ん before a consonant or at word-end, `nn` / `n'` for an explicit ん before a vowel, and — in katakana only — repeated vowels contract into the long-vowel mark.

**Why no Mandarin pinyin?** Kana conversion works because romaji → kana is a direct, one-to-one mapping. Pinyin isn't: a single syllable maps to several unrelated Chinese characters (`ma` → 妈 / 麻 / 马 / 骂, depending on tone and meaning), so a real converter needs a dictionary and a candidate picker — a genuinely different kind of tool, not an extension of this one.

### Known limitations

- **Only two levels per key.** Each layout holds a key's unshifted and shifted characters, which match the xkeyboard-config definitions for every layout. AltGr characters (German `@ € {}`, French `€ ~ #`, Spanish `@ #`) are not modelled, so text typed with AltGr passes through unconverted. Ukrainian Shift + ` gives ₴, as Microsoft's "Ukrainian (Enhanced)" layout does (xkeyboard-config has ʼ there).
- **A couple of very common Japanese words** (こんにちは, こんばんは) are spelled with は rather than phonetic わ; these are handled as explicit whole-word exceptions since no phonetic rule can derive them. They apply only when the word is the whole input: inside a sentence, `konnichiwa` is spelled phonetically (こんにちわ).
- **The key map shows unshifted legends.** A shifted character (a capital letter, a Thai shift-row mark) lights the correct key, but the key shows its unshifted characters.

Spot something off? Open an issue or PR.

## Privacy

Nothing you type is stored or sent anywhere: both conversions run in your browser. Your Mode and Paper choices are kept in the browser's local storage. The pages load their fonts from Google Fonts.

## Development

### Adding a layout

Every layout in `layouts.js` is two 47-character arrays — `unshifted` and `shifted` — lined up against the shared `SLOTS` physical-key order. Drop a new entry into the `LAYOUTS` object with your layout's characters in the same 47 positions and it shows up in both pickers and on the key map automatically. [STANDARDS.md](STANDARDS.md#layout-data) lists what a new layout ships with (tests, script detection, docs).

## Tests

The conversion engines have tests that run with Node 18 or newer and need no install:

```sh
node --test tests/*.test.cjs
```

They check that every layout covers all 47 keys with no duplicate characters, that each layout's characters survive a round trip through every other layout, the README's known words, pass-through of off-keyboard characters, and the Hepburn spelling rules. The interface is checked in Chromium and WebKit at phone, tablet and desktop widths; [STANDARDS.md](STANDARDS.md#definition-of-done) has the full definition of done.

If you change a mapping table, run the tests before opening a PR.

## Project structure

```text
index.html          Layout Fix page
romaji.html         Romaji → Kana page (its page script is inline)
style.css           shared styling: five papers, each in light and dark
theme.js            Display settings: Mode and Paper
custom-select.js    styled layout pickers (the hidden <select> keeps the value)
typewriter.js       typed-out result, key strikes and Copy (both pages)
script.js           Layout Fix wiring and the key map
layouts.js          keyboard layout tables + conversion engine
kana-data.js        Hepburn romaji → kana lookup table
kana.js             romaji → kana conversion engine
icons/              keycap logo: favicon.svg plus PNG sizes
tests/              engine tests (Node's built-in runner)
assets/preview.png  Screenshot for this README
```

## Project documents

| File | What it covers |
| --- | --- |
| [PRODUCT.md](PRODUCT.md) | Who the tools are for and what they must never claim |
| [DESIGN.md](DESIGN.md) | The visual system: tokens, type, components, motion |
| [STANDARDS.md](STANDARDS.md) | Engineering and quality rules, and the definition of done |
| [ROADMAP.md](ROADMAP.md) | Where the project is heading next, with an issue per item |
| [NOTICE.md](NOTICE.md) | Third-party font and logo-glyph licences |

## Contributing

Read [STANDARDS.md](STANDARDS.md) first: it covers the module boundaries, the rules for layout and kana data, the interface rules, and the checks a change has to pass. Ideas for what to build next are in [ROADMAP.md](ROADMAP.md).

## License

MIT — see [LICENSE](LICENSE). Third-party font licensing is in [NOTICE.md](NOTICE.md).
