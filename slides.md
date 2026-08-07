---
theme: default
title: Building AI-powered Workflows for Large-Scale TypeScript Code Refactors
info: Slides for TypedCoders 3.0, Chennai
colorSchema: light
highlighter: shiki
lineNumbers: false
aspectRatio: 16/9
canvasWidth: 1024
transition: fade
fonts:
  provider: none
  sans: Geist Variable
  mono: Geist Mono Variable
drawings:
  persist: false
---

<div class="cover-wrap">
  <div class="echo mono">Rust</div>
  <div class="eyebrow">TYPEDCODERS 3.0 · CHENNAI · 08 AUG 2026</div>
  <h1 class="cover-h1">Building AI-powered Workflows for Large-Scale <span class="hl">TypeScript</span> Refactors</h1>
  <div class="cover-sub">Dani Akash <span class="sep">·</span> Founding Engineer at BrowserOS</div>
</div>

<style>
.cover-wrap { position: relative; }
.echo {
  position: absolute; right: -0.5rem; top: -6rem;
  font-size: 13rem; font-weight: 700; color: rgba(0,0,0,0.05);
  letter-spacing: -0.05em; pointer-events: none; user-select: none; z-index: 0;
}
.cover-h1 {
  position: relative; z-index: 1; font-size: 3.4rem; font-weight: 700;
  letter-spacing: -0.03em; line-height: 1.03; max-width: 22ch; margin: 0;
}
.cover-h1 .hl { color: var(--color-signal-orange); }
.cover-sub { position: relative; z-index: 1; color: var(--color-steel); font-size: 1.15rem; margin-top: 0.6rem; }
.cover-sub .sep { color: var(--color-ash); }
</style>

---
layout: statement
---

<div class="eyebrow" style="color: var(--color-ash)">THE THESIS</div>

# Your types are only as portable as your <strong>contract</strong>.

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">The server changed languages. The TypeScript client never changed a line.</div>

---
layout: section
---

<div class="mono" style="color: var(--color-signal-orange); font-size: 5rem; font-weight: 700; line-height: 1;">01</div>

# Case study: <strong>claw-server</strong>, TypeScript to Rust

<div class="mono" style="color: var(--color-ash); margin-top: 1rem; font-size: 1rem;">packages/archive/claw-server-ts &nbsp;→&nbsp; apps/claw-server-rust</div>

---

<div class="eyebrow">Proof, not vibes</div>

## How we knew the Rust port was correct

<div style="margin-top: 2rem;">
<StatCard :items="[
  { n: '205/205', l: 'cross-server tests passing' },
  { n: '37', l: 'agent review · 10 findings fixed' },
  { n: '98', l: 'commits · about one month' },
]" />
</div>

<div class="spotlight" style="margin-top: 2rem; max-width: 62ch;">
A differential harness ran the TypeScript and Rust servers side by side, compared them semantically, and wrote the migration to-do list itself.
</div>

---

<Pill>Real · before</Pill>

## Hono RPC: the server's type <em>is</em> the client's type

```ts {all|2-3|5|6-7}
// apps/claw-app/modules/api/client.ts
import type { AppType } from '@browseros/claw-server/server'
import { hc } from 'hono/client'

const client = hc<AppType>(baseUrl)
const res = await client.api.v1.sessions.$get()
//    ^ fully typed, inferred straight from the server's routes
```

<div class="mono" style="color: var(--color-steel); font-size: 0.8rem; margin-top: 0.7rem;">git show 9271e3cef^ · the pre-migration client</div>

---

<Pill spot>The idea</Pill>

## Beautiful types. Trapped in one language.

<div class="idea-list">

- `hc<AppType>()` infers the whole API from `typeof routes`
- Zero codegen, zero drift, and it all lives in the compiler
- <strong>But `typeof routes` cannot cross into Rust</strong>

</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.4rem;">That single fact is why the contract had to leave TypeScript before the server could.</div>

<style>
.idea-list li { font-size: 1.4rem; line-height: 1.8; }
</style>
