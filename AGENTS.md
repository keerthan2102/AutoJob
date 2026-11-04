# Development guidelines

## Solutions that work across websites

This application works across different websites and automation types. Base new
features and bug fixes on browser behavior shared across websites.

- Do not fix a bug by tying the solution to the domain, URL pattern, CSS class,
  listing title, or fixed page text of the website where it was observed.
- Loading, reading, waiting, and recovery must use the same logic across
  websites. Use general DOM and accessibility information, currently observed
  controls, and the content required by the task.
- An open page does not mean the requested content is ready. Do not treat an
  incomplete load as an empty result, an unsuitable record, or a completed read.
- Waits and retries must be bounded, cancellable, and scoped to the task and
  tab. Do not repeatedly send unchanged content to the model or restart an
  ongoing load by reopening the page.
- Validate fixes against different URL formats and page structures, delayed
  content, genuinely short content, and loads that never complete. Preserve
  automations beyond job listings and avoid disrupting other workers.
- If the shared mechanism cannot retrieve the content, report that clearly.
  Do not assume missing content or a successful result. Add an exception for a
  particular website only when the user explicitly requests it.

## Language of app text

All text in the app is written in English: agent-facing text (system prompts,
tool names and descriptions, notices, guidance, action messages, error text
returned from a tool, and any evidence or summary the app composes for an
agent), the application UI, Telegram messages, questions to the user, code
comments and docs. Do not add Turkish (or any other language) strings, and
translate any you touch. The only exception is multilingual vocabulary that
recognises text on third-party pages or what a user types (for example a
regex matching a "Bewerben" or "Başvur" button alongside "Apply").

Follow [CONTRIBUTING.md](CONTRIBUTING.md) for other development and validation
steps. Keep temporary page content and the Jev cache in RAM; read pages again
after the application restarts.

## Cursor Cloud specific instructions

- Toolchain: Node.js 22.22.3+ and pnpm 10.14.0 (see `packageManager`), Rust 1.90.0 from `rust-toolchain.toml`. `pnpm bootstrap` verifies vendored TermLoop, installs dependencies, builds the renderer, and compiles the engine.
- Electron 43 does not download its binary from a postinstall script. After `pnpm install`, run `node node_modules/electron/install.js`.
- Launch the desktop app with `pnpm exec electron .` on the existing display (`DISPLAY=:1` when the desktop is up). Interface smokes on Linux need Xvfb (`xvfb-run -a`), as CI does.
- Local checks: `pnpm check`, `pnpm test`, and `cargo test --locked --manifest-path engine/Cargo.toml --target-dir engine/target`.
