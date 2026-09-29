# Interaction and copy

Adapted from the user-supplied [Interface Cheat Sheet](https://interfaces.dev/cheat-sheet). These checks concern whether people can understand and operate the interface, including with keyboard, touch, and assistive technology.

## Semantics and keyboard

- Use native elements for their intended behavior: `<button>` for actions and `<a href="…">` for navigation. Preserve link capabilities such as opening in a new tab and normal keyboard activation.
- Use `:focus-visible` for custom focus styling. Never remove an outline without a visible replacement, and retain usable focus behavior in the supported browsers.
- Keep the natural focus order. When explicit `tabindex` is necessary, use `0` or `-1`; avoid positive values.
- Give icon-only buttons a descriptive accessible name, usually with `aria-label`. Decorative icons inside a named button may be hidden from assistive technology, but never put `aria-hidden="true"` on a focusable element itself.
- Give informative images alternative text describing their purpose and relevant content. Use `alt=""` for decorative images.
- On pages with repeated navigation, make a skip-to-content link the first keyboard stop and ensure its target works.

## Forms

- Give every input a visible, associated `<label>`. Choose `type` and `inputmode` for the expected value; a placeholder is not a label.
- Allow paste, including for passwords and one-time codes.
- Keep submission available before a request starts so people can submit and learn what needs fixing. Validate on submission; do not make an invalid form's only submit control unreachable through a disabled state.
- Mark invalid fields with `aria-invalid="true"`, connect their messages through `aria-describedby`, and move focus to the first invalid field after a failed submission. Retain existing descriptions when adding error-message associations.

## Pointer, touch, and motion

- Aim for hit areas of at least `24×24px`, preferably `44×44px` for touch and `40×40px` on desktop when possible. The hit area may be larger than the visible icon; expanded hit areas must not overlap.
- Set `pointer-events: none` on noninteractive decorative layers such as glows or gradient overlays so they cannot intercept input.
- Scope hover-specific styles to `@media (hover: hover)` to avoid sticky hover appearances after a touch tap. Keep focus and pressed states available independently.
- Gate decorative animation with `@media (prefers-reduced-motion: no-preference)` or the project's equivalent motion setting. Under reduced motion, state changes and feedback must still happen without depending on animation completion.

## Feedback

- Announce routine updates through `role="status"`; reserve `role="alert"` for urgent errors that warrant interruption.
- Pair color changes with another cue, such as a label, icon, or underline. A status must remain understandable without color perception.

## Writing

- Use action verbs in button labels, such as “Save draft” or “Delete project,” instead of ambiguous “OK!” or “Yes.”
- In confirmations, name the consequence on the affirmative control, such as “Delete project,” alongside “Cancel.”
- Keep the next-step label consistent across a flow: choose “Continue” or “Next.” If the final step performs a different action, label that action accurately.
- Make link text describe the destination rather than relying on “Click here.”
- Apply capitalization consistently across labels, headings, and buttons. Sentence case is a useful default when the product has no established convention.
- Describe a toggle's enabled behavior positively, such as “Send read receipts,” rather than “Disable read receipts.”
- In an empty view, explain what belongs there and offer one relevant way to get started. Distinguish a first-use empty state from a filtered search with no matches.
- Address the reader directly as “you,” using the appropriate form in the interface's language, rather than referring to “the user.”
