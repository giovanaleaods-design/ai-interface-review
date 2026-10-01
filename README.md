# AI Interface Review

A Claude skill that reviews AI-generated interfaces and suggests concrete changes. It analyzes and critiques; it does not build new UI.

Works with screenshots, live URLs, code (HTML, CSS, JSX, Tailwind) and Figma frames.

## What it flags

- Decorative gradients, sparkle icons and emojis
- Excessive animation on hover and scroll
- Spacing and corner radius outside a consistent scale, and too many rounded corners
- Low contrast, translucent headers that vanish on scroll, information shown only by color
- Excessive helper text
- Empty slogans and inflated promises
- Purple and orange palettes, neon dark mode
- Generic fonts such as Inter and Geist
- The default "centered hero + three cards + CTA" landing layout

## What it always checks

- Empty, loading and error states (not only the happy path)
- Breakpoints from 320px to 1440px and layouts that break
- Microcopy across the whole screen

## Installation

**Claude.ai / Claude app:** zip the `SKILL.md` file inside a folder named `ai-interface-review` and upload it in Settings > Capabilities > Skills.

**Claude Code:** copy the folder to `~/.claude/skills/ai-interface-review/` (personal) or `.claude/skills/ai-interface-review/` (project).

## Usage

Share a screenshot, URL or code and ask Claude to review the interface. The output is a report with a summary, a findings table rated by severity, proposed states, breakpoint issues, a microcopy table and a list of what could not be verified.
