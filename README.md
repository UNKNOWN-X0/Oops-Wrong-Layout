# layout-fix

Typed a whole sentence in the wrong keyboard layout and forgot to switch? Paste it, pick the two layouts, get it back.

Works by mapping each character to the physical key that produced it in the "typed as" layout, then reading off what that same key produces in the "should have been" layout — the same trick tools like Punto Switcher use.

**[Live demo](#)** — replace with your GitHub Pages URL once deployed (Settings → Pages → deploy from `main`).

## Supported layouts

- English (US QWERTY)
- Thai (Kedmanee)
- German (QWERTZ)
- Russian (ЙЦУКЕН)

## Usage

Open `index.html` in a browser, or serve the folder statically — no build step, no dependencies beyond a Google Fonts import.

1. Paste the garbled text into the left box.
2. Set "Typed as" and "Should have been" to the two layouts involved.
3. Click **Convert**, or click **Auto-detect direction** if you're not sure which way round they should be.
4. **Copy result** to grab the fixed text.

## Project structure

```
index.html    page structure
style.css     styling
layouts.js    keyboard mapping data + conversion logic
script.js     UI wiring
```

## Adding a layout

Each layout in `layouts.js` is just two 47-character arrays (`unshifted` and `shifted`) lined up against the shared `SLOTS` physical key order. Add a new entry to the `LAYOUTS` object with your layout's characters in the same 47 positions, and it will appear in the dropdowns automatically — no other code changes needed.

## Known limitations

The Thai and German mappings cover the common letter/number keys accurately. A handful of rare shifted symbol keys (some Thai tone-mark shift positions, some German AltGr characters) were built from reference rather than verified on physical hardware, and may be slightly off. Open an issue or PR if you spot one.

## License

MIT — see [LICENSE](LICENSE).

The Google Fonts imported in `style.css` (Space Grotesk, JetBrains Mono) are licensed separately under the [SIL Open Font License](https://openfontlicense.org/); they're loaded from Google's CDN, not bundled in this repo.
