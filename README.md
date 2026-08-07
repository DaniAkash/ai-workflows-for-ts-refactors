# Building AI-powered Workflows for Large-Scale TypeScript Code Refactors

Slides for the talk delivered at TypedCoders 3.0, Chennai.

Two case studies: migrating a production TypeScript service to Rust while keeping the TypeScript client type-safe, and a Rust-to-TypeScript typed comms layer between a Tauri desktop app and an Expo companion.

Built with [Slidev](https://sli.dev).

## Run the deck

```sh
bun install
bun run dev
```

Then open http://localhost:3030.

## Export

```sh
bun run export   # PDF
bun run build    # static site in dist/
```

## Design

The visual system (colors, type, spacing, components) lives in [design.md](./design.md).
