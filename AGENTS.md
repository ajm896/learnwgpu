# Repository Guidelines

## Project Structure & Module Organization
- `src/lib.rs`: Core wgpu/winit app (State, App, `run()`, `run_web()` for WASM).
- `src/main.rs`: Native entrypoint calling `learnwgpu::run()`.
- `Cargo.toml`: Rust dependencies and `wasm32` feature flags.
- `pkg/`: Prebuilt WASM bundle (`learnwgpu.js`, `*.wasm`) for the web target.
- `target/`: Build artifacts (untracked).

## Build, Test, and Development Commands
- Build native: `cargo build`
- Run native: `cargo run` (optional logs: `RUST_LOG=info cargo run`)
- Format: `cargo fmt --all`
- Lint: `cargo clippy -- -D warnings`
- Run tests: `cargo test`
- Run web (WASM): serve a page that includes a canvas and imports `./pkg/learnwgpu.js`.
  Example `index.html`:
  ```html
  <canvas id="canvas"></canvas>
  <script type="module">import './pkg/learnwgpu.js';</script>
  ```

## Coding Style & Naming Conventions
- Use Rust defaults via `rustfmt`; 4‑space indentation, no trailing whitespace.
- Names: modules/functions `snake_case`, types/traits `UpperCamelCase`, constants `SCREAMING_SNAKE_CASE`.
- Errors with `anyhow`; logging via `log` macros. Keep functions small and focused.

## Testing Guidelines
- Unit tests live beside code: `#[cfg(test)] mod tests { ... }`.
- Integration tests go in `tests/` (e.g., `tests/basic.rs`).
- Name tests clearly (e.g., `it_initializes_state`). Run with `cargo test`.

## Commit & Pull Request Guidelines
- Use clear, imperative subject lines. Conventional Commits encouraged:
  `feat: ...`, `fix: ...`, `refactor: ...`, `docs: ...`, `test: ...`, `chore: ...`.
- PRs: include a concise description, rationale, and any screenshots (web rendering) if relevant. Link issues. Keep diffs focused and update docs when behavior changes.

## Web (WASM) Notes
- The web build expects a `<canvas id="canvas">`. `run_web()` is invoked via `wasm-bindgen(start)` when `pkg/learnwgpu.js` loads.
- On the web, logs go to the browser console via `console_log`.

## Security & Configuration Tips
- Don’t commit secrets or large build outputs; `target/` is ignored. Only update `pkg/` when intentionally rebuilding for web.
- When enabling new wgpu features/limits, document them in `Cargo.toml` and gate usage appropriately.
