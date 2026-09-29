# Visual foundations

Adapted from the user-supplied [Interface Cheat Sheet](https://interfaces.dev/cheat-sheet). Use the sections relevant to the current component. Visual values are defaults to evaluate in context.

## User interface

- For nested rounded shapes, aim for concentric corners: `outer radius = inner radius + gap`. Measure the actual distance between edges, including padding and borders, rather than assigning both shapes the same radius.
- Judge alignment optically. An icon or asymmetric shape may need a small adjustment even when its bounding box is mathematically centered.
- On a text-and-icon button, slightly reduce padding on the icon side when that makes the whole content look centered.
- Use layered `box-shadow` when an elevated surface needs depth. Do not replace a useful boundary or focus indicator just to remove borders.
- To define image edges, try a `1px` outline with `outline-offset: -1px`: black at `8%` opacity in light mode and white at `8%` in dark mode.
- Match the apparent weight of an icon's stroke to its neighboring text.

## Typography

- Prefer `.woff2` for web-served font assets rather than serving `.ttf` or `.otf` originals.
- Apply `font-variant-numeric: tabular-nums` to changing timers, counters, prices, and numeric table cells so digit changes do not move the layout. A monospace font already provides equal-width digits.
- Aim for roughly `60–75` characters per line in articles and other long text. A `ch`-based maximum width is a starting point; verify the actual font and viewport.
- Use `text-wrap: balance` for headings and `text-wrap: pretty` for short descriptions when useful. Avoid applying either across long-form text by default.
- Keep long words, URLs, and identifiers within their container with `overflow-wrap: break-word`. Short labels and badges may use `white-space: nowrap`, provided they still fit at narrow widths and with translated text.
- Where it fits the existing typography, evaluate `-webkit-font-smoothing: antialiased` and `-moz-osx-font-smoothing: grayscale` on the root. These are platform-specific rendering choices, not a universal readability improvement; visually check the resulting weight.
- Store copy in its normal capitalization. Apply `text-transform` when uppercase or lowercase is only a presentation choice.
- Use typographic punctuation where the product's writing conventions permit it: curly quotes, en dashes for ranges, em dashes for asides, and an ellipsis character. Do not override a project's prohibition on em dashes or transform code and identifiers.
- Try `text-underline-position: from-font` with `text-decoration-skip-ink: auto` so underlines avoid descenders.
- If visible text is truncated with an ellipsis, provide access to the full value through an expanded view or an accessible tooltip. A hover-only reveal leaves keyboard and touch users without the full text.

## Colors and themes

- Give palette steps an actual job, such as page background, hover surface, border, solid fill, or body text. Avoid unused steps added for symmetry.
- Reference semantic tokens from components, such as `--color-text-secondary`; map raw primitives such as `--blue-500` to those tokens in the theme layer.
- Name new tokens by purpose rather than current hue or an incidental location. `--color-accent-solid` can survive a brand color change better than `--color-blue-button`.
- In a new naming scheme, reserve `accent` for brand color and distinguish `text-primary` from an accent fill. In an existing system, preserve established names and remove ambiguity through its existing conventions.
- Evaluate contrast against the surface immediately behind the text or control, including overlays and nested cards, rather than only the page background.
- Design a separate dark palette; simple inversion of the light palette rarely preserves hierarchy and contrast.
- Keep one resolved theme state governing component colors. Media-query-driven or class-driven styling can work; avoid competing rules that render different elements in different themes. A system preference may feed a class-based theme resolver without becoming a second styling authority.
- Choose gradient interpolation deliberately: `in oklab` for smoother perceived lightness, `in oklch` for more vivid intermediate colors, or `in srgb` for more muted midtones. Check the actual stops and target-browser support.

## Layout

- Give anchor targets `scroll-margin-top` when a sticky header or surrounding spacing would otherwise obscure the target heading.
- Make group boundaries clearer than item spacing. A useful starting ratio is at least `2:1`: for example, `8px` between related items and `16px` or more between groups.
