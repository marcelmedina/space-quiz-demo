# Copilot instructions

## Project shape

The entire app is a single self-contained `index.html`: inline `<style>`, inline `<script>`, no build step, no package manager, no server, and no external assets or CDNs. Keep it that way. Do not add dependencies or split files unless asked.

## Running and testing

- Run the app by opening `index.html` directly in a browser (`file://` URL). The integrated browser canvas works.
- There is no test suite, linter, or CI. Do verification as a browser-level smoke test: run JavaScript in the page that dispatches `keydown` events on `document`, clicks `.option` / `#next` / `#restart`, and checks DOM state such as `#score`, `#score-sr`, `#meta[data-status]`, `#bar` width, `.results .big`, and `.results .emoji`.
- Reload the page before each smoke-test run, and again afterwards so the user gets a fresh quiz.
- For real keyboard input (Tab/Enter focus order) instead of simulated events, use the Playwright MCP server configured in `.github/mcp.json`. Navigate to the `file:///…/index.html` URL.

## Architecture (inside `index.html`)

- **Data:** the `QUESTIONS` array holds `{ q, a: [4 choices], c: correctIndex }`. Questions are shown in order and not shuffled. Results thresholds in `results()` assume 10 questions and use the score fraction `pct`.
- **State:** two module-level variables, `index` and `score`. There is no framework.
- **Rendering:** `render()` rebuilds `#card` with `innerHTML` for each question and re-binds listeners. `choose(i)` mutates the current DOM in place to show feedback. `next()` moves forward and calls `results()` after the last question. "Play again" resets `index` and `score`, then calls `render()`.
- **Keyboard:** one global `keydown` handler maps A–D and 1–4 to `.option` buttons. It ignores Ctrl/Alt/Meta combinations and key repeats, and does nothing when no enabled `.option` exists (for example after answering, or on the results screen).

## Conventions

- **Theming:** use only the CSS custom properties in `:root`. Each one has a dark override in `@media (prefers-color-scheme: dark)`. Any new colour must be defined in **both** blocks. Don't hard-code colours in components, except white text on accent fills.
- **Feedback states:** `.option.correct` (green + `pop` animation), `.option.wrong` (red + `shake`), `.option.dim` for the other choices, `.feedback.good` / `.feedback.bad`, and `#meta[data-status="correct"|"wrong"]` to drive the status light (`--led`).
- **Mission-control status bar (`.meta`):** the visual readout (zero-padded with `pad()`, monospace, uppercase labels) is `aria-hidden`. A parallel `.sr-only` span carries the plain-language text. When updating the score, update both `#score` and `#score-sr`.
- **Accessibility:** `#card` is `aria-live="polite"`. The progress bar has `role="progressbar"`, and `aria-valuenow` must be updated together with the `#bar` width. Focus moves to `#next` after an answer. All animations are disabled under `prefers-reduced-motion`.
- **Typography:** system sans-serif stack, 15px body (keep body text in the 14–16px range), and a narrow centred column (`.wrap`, max-width 440px).
