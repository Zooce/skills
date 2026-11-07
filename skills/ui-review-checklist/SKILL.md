---
name: ui-review-checklist
description: >
  Review a change that alters what a user sees or does, such as layout,
  flows, UI copy, or CLI prompts and messages, using evidence from the
  real UI. Use before approving a user-facing change, or when the user
  runs /zooce:ui-review-checklist.
---

# UI review checklist

Review against the brief or spec, or the stated intent if neither exists.

## 1. Evidence

- Get screenshots of a real build at 390px and 1280px wide. Cover every changed state:
  - default
  - empty
  - loading
  - error
  - success
  - disabled
  - signed in and signed out, where relevant
  - long or edge-case content, such as long names and maximum lengths
- For a CLI, get the real terminal output for each changed prompt, message, and error.
- A state that you cannot see is not verified.
- For flows that depend on timing, auth, or network, prefer a run on a deployed test environment.

## 2. Behavior

- Every state, copy string, and behavior in the spec is there. Copy matches word for word, punctuation included.
- Empty, loading, and error states say what happened and what to do next.
- No wrong state flashes, such as signed-out UI shown to a signed-in user, or stale content before fresh content.
- The layout does not shift when the state changes. Reserve the space.
- Destructive actions ask for confirmation or offer Undo.
- Unsaved work is never lost without warning.
- Status shows only when it is true. For example, show "Saved" only after the server confirms.

## 3. Accessibility

- Everything works with Tab, Shift+Tab, Enter, Space, and Esc. The focus order is logical and focus is visible. Focus moves correctly when dialogs and menus open and close.
- Touch targets are at least 44×44px on mobile.
- Contrast meets WCAG AA: 4.5:1 for text, and 3:1 for large text and UI components.
- Controls have accessible names. Decorative icons and spinners are `aria-hidden`.
- Status changes go to a live region (`role="status"`) that is always in the page and then receives text. Do not insert an element that already holds its text. Nothing is announced twice, and routine page loads are silent.
- Color is never the only signal.
- Motion respects the reduced-motion setting.

## 4. Copy

- Copy is plain, short, and specific. It says what happened and what to do next. No jargon or internal terms.
- The same thing has the same name everywhere.
- Error messages never blame the user and never show internals such as codes or stack traces.
- A copy change that affects the brand needs the product owner's approval.

## 5. Report

Use the buckets from `code-review`. Omit an empty bucket. Always include Act On.

- **Act On.** Broken flows, wrong states, accessibility failures, and spec mismatches.
- **Consider.** Clarity and consistency issues.
- **Notes.** Optional polish.

For each finding, give the screen or state, what is wrong, and the exact fix. Approve only when Act On is empty and the evidence covers every changed state.
