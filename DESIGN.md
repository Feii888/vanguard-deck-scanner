# Design principles: Vanguard Deck Scanner

## Contrast colour: split-complementary, lightest colour dropped

Use this rule whenever a control needs a colour that contrasts with the main accent, such as the arrow
part of a ticked **Compare** button. It comes from the guide *Why one color looks brightest in a
split-complementary scheme* (Nov 23, 2026).

1. **Start from the base colour:** the accent the contrast sits next to.
2. **Build its split-complementary scheme:** the base hue plus the two hues 30° either side of its opposite,
   that is base + 150° and base + 210° on the HSL colour wheel, at the base's saturation and lightness.
3. **Measure the lightness of all three** as CIE L\* (0 = black, 100 = white). Compute L\* from sRGB relative
   luminance, Y = 0.2126 R + 0.7152 G + 0.0722 B on linearised values; the wheel's even spacing says nothing
   about lightness.
4. **Drop the lightest of the three.** It looks brightest and pulls the eye away from everything else.
   With a blue base, the lightest is the yellow-green split.
5. **Use the remaining split hue** as the contrast colour.
6. **Give it the same perceived lightness as the base:** match OKLCH L, then lower the chroma until the colour
   is inside sRGB. The two colours then differ in hue, not brightness. Expect less chroma on naturally light
   hues (yellow, green).

**Exception: blue is answered with red.** A blue base always gets a red hue (OKLCH hue about 20–29°) instead
of the split hue from step 5. Blue against red is the familiar pairing from games and team colours.
Steps 3 and 6 still apply.

**Black and white (neutral) schemes** have no hue to split. Apply the same lightness test to the scheme's three
tones, for example black, a mid grey and white: drop the lightest, and use the remaining tone that stands
furthest in lightness from the element it sits next to.

**Checking a palette:** view it in greyscale. The colour that turns into the lightest grey is the one that
feels brightest.

### Current values

The colours live as CSS tokens at the top of `index.html`; `--accent-2` is the contrast colour.

| Theme | Base accent | Split +150° | Split +210° | Dropped (lightest) | Contrast colour (`--accent-2`) |
|---|---|---|---|---|---|
| Light | `#2343C4` blue 228°, L\* 35 | `#C45323` red-orange 18°, L\* 50 | `#94C423` chartreuse 78°, L\* 73 | chartreuse | `#A30319` red, L\* 34, white icon |
| Dark | `#8099FF` blue 228°, L\* 65 | `#FFA680` red-orange 18°, L\* 76 | `#D9FF80` chartreuse 78°, L\* 95 | chartreuse | `#F17770` red, L\* 64, dark icon |

Without the blue exception, the rule would give the red-orange split. With it, the contrast is a true red at the
accent's own lightness (OKLCH L 0.452 light, 0.709 dark; hue 25°).

If the accent ever changes hue, rerun steps 1–6. Keep the blue-to-red exception for any blue accent, and
update this table and the comment above the `:root` tokens in `index.html`.

## App icon: "4+" (gold plate, polished blue Triple Drive)

The browser tab, the logo in the page header, the iPhone and Android Home Screens and the Mac app all use one
icon, chosen by the user on Nov 24, 2026.

- **Emblem.** The Compare symbol's woven Triple Drive in an upright hexagon. The whole hexagon is gold, and the
  symbol is blue.
- **Painted metal, not flat colour.** Both metals are drawn in a "non-metallic metal" way: bands from a
  near-white highlight through the full colour to a deep "horizon" band across the middle, then lighter again.
  - The gold runs pale cream to gold to ochre to deep bronze and back. The blue runs pale ice blue to royal blue
    to deep navy and back.
  - The gold and the blue share one light direction (top left), so the reflections line up.
  - Dark edges shape the symbol. A dark line and a pale line just inside the hexagon's edge read as a bevel.
  - Small white star glints sit where the light hits.
- **Weave gaps** are cut only where one strand passes over another, using masks, so the gaps' edges follow the
  strand underneath.
- **Tile.** On Home Screens and the Mac, the emblem sits at 75% of a navy square (#263062 to #0F1328, top to
  bottom). Phones round the corners themselves. The Mac and "any" icon is a rounded square with a transparent
  margin.
- **Colour rule exception.** Gold is the light yellow that the contrast rule above normally drops. Here it's
  the user's deliberate choice for the icon only; controls inside the app keep the contrast red.

| File in `design/app-icon/` | Used for |
|---|---|
| `tab-icon.svg` | Browser tab, written into the `<head>` of `index.html` as SVG, with `tab-icon-32.png` as a fallback. Also the header logo: `<symbol id="app-mark">` in `index.html`, with its ids prefixed `am-` |
| `app-icon-tile.svg` → `app-icon-tile-1024.png` | iPhone Home Screen (`apple-touch-icon.png`), Android maskable icon |
| `app-icon-rounded.svg` → `app-icon-rounded-1024.png` | Mac app (`AppIcon.icns`), the manifest's "any" icons |

The three SVGs are written by `tools\make-app-icon-svgs.ps1`, which keeps the colours and shapes in one place.
To change the icon, edit that script and run it. Then run `tools\render-icons.ps1`, which renders the PNGs with
Edge and rewrites the tab icon and the header logo in `index.html`. Then run `build-site.ps1`.

## Compare symbol: "H2 · B"

The red part of a ticked **Compare** button and the bottom **Compare** button carry this symbol. It's an
original redrawing inspired by Cardfight!! Vanguard's Triple Drive icon, not a copy of Bushiroad's artwork.

- **Three woven strands.** The centre strand runs straight up. The outer two start as legs on one side, sweep
  across and end as the arrow on the other side. One outer strand passes over the centre, and the centre passes
  over the other, with small gaps showing what's on top. Meaning: three drives, stronger together.
- **Arrows head the same way.** The side arrows lean about 14° in toward the centre arrow, never away from it.
- **Bold, solid shapes.** Square-cut strokes 2.4 units wide in a 24-unit box, with solid triangular heads. "B"
  has heads about 30% bigger than the first draft; the side strands' tops sit 0.8 units further out, so the heads
  keep a 0.7-unit gap.
- **Frame "H2".** A solid hexagon filled to its edge, with a fine line cut in just inside the edge. The whole
  emblem is turned 45° to the right, so the arrows point up-right.
- **Colours.** The hexagon takes the button's ink colour (`--accent-2-ink`). The symbol and cut line take the
  button's own colour (`--ko` = `--accent-2`), so they read as cut out of the hexagon.
- **Size.** 26 px in a 46 px wide button part (54 px on touch screens), 24 px in the bottom button. At 20 px the
  over-and-under gaps get too fine.

It lives once in `index.html` as `<symbol id="td-mark">` at the top of `<body>`. The design files are in
`design/compare-symbol/` in the project folder on the PC. They aren't part of the website upload.

| File | What it is |
|---|---|
| `H2-B (in use).svg` | The symbol in use |
| `H2.svg` | Same frame, first-draft arrowheads (the side strands' tops at their original spacing) |
| `H2-A.svg` | Arrowheads about 15% bigger, side strands' tops 0.6 units further out |

Each file shows the emblem on the light-theme red and the dark-theme red, plus the symbol upright.
