---
name: interface-cheat-sheet
description: "Design, refine, or review web interfaces using practical criteria for visual polish, typography, color, motion, accessibility, and UI copy. Use for component or page implementation and focused interface improvements; not for backend work or brand strategy alone."
---

# Interface Cheat Sheet

Make interfaces clearer, easier to operate, and visually coherent using the [Interfaces cheat sheet](https://interfaces.dev/cheat-sheet), adapted from the user's supplied Markdown.

## Apply the guidance

Start with the affected interface and its existing components, tokens, styles, and interaction states. Reuse the project's conventions. Identify the actual usability or visual problem before changing code.

Read the relevant reference and sections:

| Concern | Reference |
| --- | --- |
| Alignment, radii, depth, typography, colors, themes, spacing | [Visual foundations](references/visual-foundations.md) |
| Entrances, exits, press feedback, icon swaps, interrupted transitions | [Motion](references/motion.md) |
| Semantics, keyboard use, forms, touch, reduced motion, feedback, labels | [Interaction and copy](references/interaction-and-copy.md) |

For a whole interface review, use all three. For a narrow change, load only what bears on it. The local references contain the working guidance; fetching the source is unnecessary unless the task asks for updates.

Treat visual recipes and their numerical values as starting points. Keep deliberate product choices and existing design-system equivalents when they serve the same purpose. Do not rename an established token system, add animation, or redesign unrelated components merely to match an example. Accessibility and usable behavior take precedence over decorative effects. Honor project copy conventions, including punctuation and localization.

When implementing, make the smallest coherent change that fixes the observed issue. Check the states the change affects, such as focus, hover, press, loading, invalid input, long content, or theme switching. Preserve reduced-motion behavior when adding or changing motion.

When reviewing, report concrete issues supported by the supplied code or rendered UI. Explain the user impact and suggest a specific correction. Do not manufacture a finding for every heuristic or describe an aesthetic preference as a functional defect. Make changes only when the request authorizes implementation.

Verify the affected behavior with the available preview or the narrowest relevant check. A screenshot can support alignment and hierarchy judgments; keyboard operation, validation, interrupted motion, and announcements need interaction or code evidence. State what was checked and what remains unverified.
