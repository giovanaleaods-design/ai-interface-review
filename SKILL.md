---
name: ai-interface-review
description: Review AI-generated interfaces (screenshots, URLs, code or Figma) and suggest concrete changes. Analyzes and critiques only; does not build new UI.
---

# AI Interface Review

Use this skill to review an interface that was generated (fully or partly) by an AI tool and to point out what makes it look generic, inconsistent, inaccessible or incomplete. The goal is analysis and specific recommendations. Do not redesign the screen from scratch and do not deliver a new full interface unless the user explicitly asks for it. Short code or copy snippets that illustrate a fix are fine.

## Inputs

The interface can arrive as:
- Screenshots or images
- A live URL
- Code (HTML, CSS, JSX/TSX, Tailwind classes, design tokens)
- Figma frames or exported specs

Work with whatever is available. When only a screenshot is provided, say which checks could not be fully verified (real breakpoints, animation, hover states, exact contrast values) and mark those findings as "to verify".

## Process

1. **Understand the context first.** Identify the product type, the target user and the main task of the screen. A finding only matters if it hurts that task, the brand or accessibility.
2. **Run the AVOID checks** (below). Note each occurrence with its location on the screen.
3. **Run the ALWAYS checks** (below). These are mandatory in every review, even if the user only asked about visuals.
4. **Rate severity** for each finding:
   - **Critical**: blocks use or fails accessibility (contrast, color-only status, broken layout).
   - **Major**: makes the interface look generic or inconsistent, or confuses the user.
   - **Minor**: polish and refinement.
5. **Write the report** using the output format at the end of this file.

## AVOID: patterns to flag

### 1. Gradients, sparkle icons and emojis
- Flag decorative gradients on backgrounds, buttons, text and borders. In code, look for `linear-gradient`, `radial-gradient`, `bg-gradient-to-*`, `bg-clip-text`.
- Flag the sparkle icon (✨ or similar) used to signal "AI" or "magic". Suggest a label that names the actual action instead ("Summarize", "Generate draft").
- Flag emojis in UI labels, headings, buttons, empty states and toasts.
- Suggest flat, solid color with intent.

### 2. Excessive animation
- Flag hover effects that scale, lift, glow or rotate cards and buttons. A color or underline change is usually enough.
- Flag scroll-triggered fade-ins, parallax, staggered reveals and content that only appears after scrolling.
- Flag animations longer than ~300ms on UI feedback, and any animation without a `prefers-reduced-motion` fallback.
- Rule of thumb: motion should explain a change of state (open, close, loading, success). If it only decorates, recommend removing it.

### 3. Irregular spacing and corner radius
- Spacing must follow one scale (for example a 4px or 8px base: 4, 8, 12, 16, 24, 32, 48, 64). Flag values outside the scale (13px, 18px, 27px) and inconsistent gaps between equivalent elements.
- Corner radius must come from a small, defined set (for example 0, 4, 8). Flag mixed radii on sibling components and nested elements whose inner radius does not relate to the outer one.
- In code, list every distinct spacing and radius value found and show which ones break the scale.

### 4. Too many rounded corners
- Flag large radii (16px and above) applied everywhere: cards, inputs, modals, images, buttons. Pill shapes should be reserved for chips, tags and toggles.
- Recommend tighter, more restrained radii that match the product tone.

### 5. Accessibility and legibility
- **Low contrast**: body text must reach at least 4.5:1, large text and UI components (borders, icons, focus rings) at least 3:1 (WCAG 2.2 AA). Report the measured or estimated ratio. Pay special attention to gray-on-gray secondary text and placeholder text.
- **Translucent header that disappears on scroll**: flag `backdrop-filter` / glassmorphism headers whose text loses contrast over content, and headers that hide on scroll and take the navigation with them.
- **Color-only information**: flag status, validation, charts, tags or required fields that rely only on color. Each one needs a supporting element: icon, text label, pattern or shape.
- Check visible focus states and minimum touch targets (44x44px).

### 6. Excessive helper text
- Flag helper text under every field, instructional paragraphs before simple forms and tooltips that repeat the label.
- Keep helper text only where the user would otherwise make a mistake (format requirements, consequences of an action, constraints).

### 7. Empty slogans and inflated promises
- Flag hollow taglines such as "Create without limits", "The future of your business", "Unleash your potential", "Supercharge your workflow", "Revolutionize the way you work".
- Flag overconfident claims ("10x faster", "never worry again", "perfect results every time") without proof.
- Suggest copy that says what the product does, for whom, and the concrete outcome.

### 8. Purple and orange
- Flag purple and orange as primary or accent colors, including violet, indigo-violet, magenta-purple and saturated orange. They read as the default AI-generated palette.
- Suggest deriving the palette from the brand, the domain or the content instead, with one clear accent color.

### 9. Neon dark mode
- Flag dark themes with neon or highly saturated accents (electric cyan, lime, hot pink, glowing purple) and glow effects (`box-shadow` with colored blur, `text-shadow` glows).
- Recommend dark surfaces in layered, slightly tinted grays, desaturated accents and checked contrast for both text and accents.

### 10. Generic fonts
- Flag Inter, Geist and other default system-like choices when used as the only typeface without intent. In code, check `font-family` and font imports.
- Recommend a typeface with character that fits the product's tone, and a clear type scale with defined weights, sizes and line heights. Flag more than 2 families or more than 3 to 4 weights in use.

### 11. The default landing layout
- Flag the pattern: centered symmetrical hero with headline + subheadline + two buttons, followed by a row of three equal feature cards with icons, followed by a CTA band.
- Suggest layouts driven by the actual content: show the product itself, use asymmetry, real screenshots or data, varied section rhythm and a hierarchy that reflects what matters most to this user.

## ALWAYS: mandatory checks

### A. States beyond the happy path
For every main component or view that loads, lists, searches or submits data, propose options for:
- **Empty state**: first use (nothing created yet), no results (search or filter), and cleared state. Each one should explain what happened in one line and offer the next action.
- **Loading state**: skeletons that match the final layout for content, inline spinners for actions, progress indication for long tasks, and what happens if loading takes longer than expected.
- **Error state**: what failed, why (if known), and how to recover (retry, edit, contact). Cover field validation, failed submit, network loss, permission denied and partial failure.

Give 2 options per state when the right choice depends on context. Include the proposed microcopy.

### B. Breakpoints and broken layouts
Check the interface at least at 320, 375, 768, 1024, 1280 and 1440px.
- With a URL and a browser tool available: resize and inspect each width.
- With code: read the media queries, container widths, fixed sizes and overflow rules.
- With screenshots only: point out the elements most likely to break and mark them "to verify".

Look for: horizontal scroll, text overflow and truncation, tables without a mobile strategy, fixed-width elements, images without aspect-ratio control, navigation that does not collapse, buttons wrapping awkwardly, modals taller than the viewport, sticky elements covering content, and long translated strings (for example Portuguese or German text is often 30% longer than English).

### C. Microcopy review
Review every visible string: headings, buttons, labels, placeholders, tooltips, toasts, empty and error messages.
- Buttons start with a verb and name the result ("Save changes", not "Submit" or "OK").
- Use the user's vocabulary, not internal or technical terms.
- Keep tone consistent across the screen.
- Placeholders are not labels.
- Error messages never blame the user and always offer a way forward.
- Remove filler, repetition and marketing language inside the product UI.

Present microcopy fixes as a table: current text, suggested text, reason.

## Writing style for the report

- Be direct and specific. Name the element, the problem and the fix.
- Avoid generic praise, filler and AI-sounding phrasing.
- Do not use em dashes.
- Each recommendation must be actionable: a value, a token, a piece of copy or a concrete layout change.
- Mention what is working well briefly, only when it helps the team know what to keep.

## Output format

```
# Interface review: <screen or product name>

## Summary
2 to 4 sentences: overall assessment and the top 3 changes with the biggest impact.

## Findings
| # | Severity | Category | Where | Problem | Suggested change |
|---|----------|----------|-------|---------|------------------|

## States (empty, loading, error)
For each relevant view: options with description and microcopy.

## Breakpoints
Width by width: what breaks and how to fix it. Mark unverified items.

## Microcopy
| Current | Suggested | Reason |
|---------|-----------|--------|

## Not verified
Checks that could not be completed with the material provided, and what is needed to complete them.
```
