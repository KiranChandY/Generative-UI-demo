# Design tokens

`generative-ui.tokens.json` is the Generative UI design system as [W3C DTCG](https://tr.designtokens.org/format/) design tokens. It holds the same values the app uses in `public/index.html`.

## What's inside

| Set | Contents |
|---|---|
| `base/color` | Raw palette: neutrals, blue, green, amber, red, purple and alpha borders |
| `base/spacing` | 2 to 96px scale (`spacing.25` … `spacing.900`) |
| `base/radius` | `none`, `sm` 4, `md` 6, `lg` 8, `xl` 12, `2xl` 16, `full` |
| `base/stroke` | Hairline (1) and focus ring (2) widths |
| `base/typography` | Geist and Geist Mono, a size scale, three weights, and 24 composite text styles (`text.heading-1` … `text.caption-5`, `text.code`) |
| `semantic/dark` | Meaning layer for the dark theme: `color.bg.*`, `color.text.*`, `color.border.*`, `color.accent.*`, `color.action.*`, `color.status.*`, `color.chart.*` |
| `semantic/light` | The same names, pointing at light values |

Two themes, **Mode / Dark** (default) and **Mode / Light**, each turn on all base sets plus one semantic set. Components should bind to the semantic tokens so switching the theme restyles everything.

## Import into Penpot

1. Open your Penpot file (Penpot 2.10 or newer, for typography tokens).
2. Open the **Tokens** tab in the left sidebar.
3. Open the tokens menu, choose **Import**, and pick `generative-ui.tokens.json`.
4. In the theme selector, choose **Dark** (or **Light**).

Then add the Geist and Geist Mono fonts to your Penpot team (both are free on Google Fonts) so the typography tokens render correctly.
