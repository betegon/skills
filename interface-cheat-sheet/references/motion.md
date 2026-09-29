# Motion

Adapted from the user-supplied [Interface Cheat Sheet](https://interfaces.dev/cheat-sheet). Add motion only when it clarifies an interaction or provides useful feedback. For reduced-motion and input-modality handling, read the relevant sections of [Interaction and copy](interaction-and-copy.md).

## Origins and timing

- Have a popover, menu, or other triggered element emerge from its trigger. Set `transform-origin` from the trigger's position and account for placement changes.
- For menus opened repeatedly, consider an immediate opening and only a brief exit animation. Frequent access should not wait for visual ceremony.
- Keep repeated feedback, such as a navigation item's hover color change, instant or very fast.
- Make exits quieter than entrances: reduce travel distance, fade opacity, and optionally use a small blur around `4px`. Blur is a visual option, not a requirement.
- When an intentional entrance contains several sections, stagger small meaningful groups with short delays instead of moving one giant block or delaying every individual item.
- Prevent first-render animations unless an entrance on page load is deliberately part of the design.

## Interaction recipes

- For press feedback, try a button scale of `0.95–0.98` with `transition: scale 200ms ease-out`. Tune the amount to the button's size and existing motion style.
- When swapping icons, crossfade them. A starting recipe for the incoming icon is scale `0.25 → 1`, opacity `0 → 1`, and blur `4px → 0`; reverse those values for the outgoing icon. Avoid geometry changes during the swap.
- Prefer CSS transitions for state changes that should reverse smoothly when interrupted. Use keyframes for deliberate one-shot sequences.
- List exact transition properties. Avoid `transition: all`, which can animate unrelated layout or style changes.
- Suppress transitions during the light/dark theme update so intermediate colors do not sweep across the page. Keep suppression scoped to the update and restore normal interaction transitions afterward.
- If an animated element shows a reproducible `1–2px` rendering shift, evaluate `will-change: transform` on that element, particularly in iOS Safari. Confirm that it fixes the observed issue; do not apply it to every element preemptively.
