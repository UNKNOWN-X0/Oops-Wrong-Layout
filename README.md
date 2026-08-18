# layout-fix

Two small typing tools, one static site.

**[Live demo](#)** — replace with your GitHub Pages URL once deployed (Settings → Pages → deploy from `main`).

## Layout Fix (`index.html`)

Typed a whole sentence in the wrong keyboard layout and forgot to switch? Paste it, pick the two layouts, get it back.

Works by mapping each character to the physical key that produced it in the "typed as" layout, then reading off what that same key produces in the "should have been" layout — the same trick tools like Punto Switcher use.

### Supported layouts

- English (US QWERTY)
- Thai (Kedmanee)
- German (QWERTZ)
- Russian (ЙЦУКЕН)
- Ukrainian (ЙЦУКЕН)
- French (AZERTY)
- Spanish (QWERTY ES)
- Hebrew

## Usage

Open `index.html` in a browser, or serve the folder statically — no build step, no dependencies beyond a Google Fonts import.

1. Paste the garbled text into the left box.
2. Set "Typed as" and "Should have been" to the two layouts involved.
3. Click **Convert**, or click **Auto-detect direction** if you're not sure which way round they should be.
4. **Copy result** to grab the fixed text.

## Romaji → Kana (`romaji.html`)

Type Japanese in romaji, get hiragana or katakana. Handles the standard Hepburn typing conventions: doubled consonants (`kitte` → きって), ん before a consonant or at word-end, `nn`/`n'` for an explicit ん before a vowel, and — in katakana — contracts repeated vowels into the long-vowel mark (`kyuuto` → キュート).

**Mandarin pinyin isn't included.** Kana conversion works because romaji-to-kana is a direct, unambiguous mapping. Pinyin isn't: a single syllable maps to many unrelated Chinese characters (`ma` → 妈/麻/马/骂, among others), so converting it properly needs a dictionary and a way to pick between candidates — a genuinely different tool, not an extension of this one.

## Project structure

```
index.html      Layout Fix page
romaji.html     Romaji → Kana page
style.css       shared styling (both pages)
theme.js        shared light/dark/system toggle (both pages)
layouts.js      keyboard mapping data + conversion logic
script.js       Layout Fix UI wiring
kana-data.js    Hepburn romaji → kana lookup table
kana.js         romaji → kana conversion engine
```

## Adding a layout

Each layout in `layouts.js` is just two 47-character arrays (`unshifted` and `shifted`) lined up against the shared `SLOTS` physical key order. Add a new entry to the `LAYOUTS` object with your layout's characters in the same 47 positions, and it will appear in the dropdowns automatically — no other code changes needed.

## Known limitations

The Thai, German, Ukrainian, French, and Spanish mappings cover the common letter/number keys accurately. A handful of rare shifted symbol keys (some Thai tone-mark positions, German/French AltGr characters, Spanish accent/dead-keys) were built from reference rather than verified on physical hardware, and may be slightly off.

Hebrew has no letter case, so converting mixed-case Latin text through Hebrew and back loses capitalization (`Hello` and `hello` both land on the same Hebrew keys) — this reflects the real keyboard, not a bug. The same applies to a few US shift-symbols (`:` `"` `<` `>`) whose physical keys are reassigned to Hebrew letters — they're simply not reachable from Hebrew mode, same as on a real Hebrew keyboard.

A handful of rare Thai shift-row symbols use placeholder Thai punctuation marks (๛ ๏ ๚ ๎) instead of their true values, which weren't reliably known — this keeps every key round-trip-safe rather than guessing and risking silent data loss.

Open an issue or PR if you spot a mapping error.

## License

MIT — see [LICENSE](LICENSE).

The Google Fonts imported in `style.css` (Space Grotesk, JetBrains Mono) are licensed separately under the [SIL Open Font License](https://openfontlicense.org/); they're loaded from Google's CDN, not bundled in this repo.
