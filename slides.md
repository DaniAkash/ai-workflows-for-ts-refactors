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

<!--
Hi, I am Dani. This is a TypeScript talk that spends a lot of time in Rust. Stay with me.
-->

---
layout: two-cols
layoutClass: gap-12
---

<div class="eyebrow">Introductions</div>

<h1 style="font-size: 3rem; margin: 0.4rem 0 0 0;">Hi, I'm <strong>Dani</strong>.</h1>

<div style="font-size: 1.4rem; margin-top: 0.4rem;">Founding Engineer at BrowserOS <span style="color: var(--color-ash)">·</span> YC S24</div>

<div style="font-size: 1.2rem; color: var(--color-steel); margin-top: 1.6rem; max-width: 30ch; line-height: 1.5;">I build browsers, terminals, and AI assistants. The seams where humans and agents meet.</div>

::right::

<div class="card meproj-card">
  <div class="card-k">Open source</div>
  <div class="meproj"><div class="meproj-n">BrowserOS</div><div class="meproj-d">Open-source agentic browser.</div></div>
  <div class="meproj"><div class="meproj-n">Agent Terminal</div><div class="meproj-d">A terminal where coding agents are first-class.</div></div>
  <div class="meproj"><div class="meproj-n">Herbie</div><div class="meproj-d">One command center for every AI agent.</div></div>
  <div class="meproj"><div class="meproj-n">acpx-tools</div><div class="meproj-d">Headless ACP toolkit for driving agents.</div></div>
  <div class="mono meproj-link">github.com/DaniAkash</div>
</div>

<style>
.meproj-card { display: flex; flex-direction: column; gap: 0.9rem; }
.meproj-n { font-weight: 600; font-size: 1.2rem; letter-spacing: -0.01em; }
.meproj-d { color: var(--color-steel); font-size: 0.95rem; line-height: 1.35; }
.meproj-link { font-size: 0.8rem; color: var(--color-signal-orange); margin-top: 0.3rem; }
</style>

<!--
A quick hello. I work on the seams where humans and agents meet: browsers, terminals, assistants. Two of these, BrowserOS and Agent Terminal, show up later as the case studies.
-->

---

<div class="eyebrow">May 2026</div>

## Bun rewrote itself in <strong>Rust</strong>.

<div style="font-size: 1.3rem; color: var(--color-steel); margin-top: 0.6rem; max-width: 54ch;">Half a million lines of Zig, ported in 11 days, almost entirely by a fleet of Claude agents.</div>

<div class="grid grid-cols-2 gap-4 bun-how" style="margin-top: 1.5rem;">
  <div class="card" v-click>
    <div class="card-k">Parallelism</div>
    <div class="card-t">~64 agents at once</div>
    <div class="card-b">Claude instances across 4 git worktrees. 6,502 commits.</div>
  </div>
  <div class="card" v-click>
    <div class="card-k">A patterns map</div>
    <div class="card-t">PORTING.md</div>
    <div class="card-b">Every Zig idiom mapped to its Rust twin, so the port stayed mechanical.</div>
  </div>
  <div class="card" v-click>
    <div class="card-k">Adversarial review</div>
    <div class="card-t">Assume it is wrong</div>
    <div class="card-b">A second agent saw only the diff and tried to break it.</div>
  </div>
  <div class="card" v-click>
    <div class="card-k">The oracle</div>
    <div class="card-t">The test suite</div>
    <div class="card-b">Bun's TypeScript tests had to be 100% green in CI before merge.</div>
  </div>
</div>

<div class="caption">bun.com/blog/bun-in-rust</div>

<style>
.bun-how .card { padding: 0.85rem 1rem; }
.bun-how .card-t { font-size: 1.1rem; margin-bottom: 0.25rem; }
.bun-how .card-b { font-size: 0.88rem; line-height: 1.32; }
</style>

<!--
Bun is a JavaScript runtime, half a million lines of Zig. A few months ago they rewrote the whole thing in Rust in eleven days. Here is HOW they did it with AI. [click through the four cards] Parallel agents, a patterns map so the port was mechanical, an adversarial reviewer told to assume the code is wrong, and the existing test suite as the pass-or-fail oracle. Hold onto these four. Our story uses the same moves.
-->

---
layout: statement
---

<div class="eyebrow" style="color: var(--color-ash)">The twist</div>

# We did the same to a production <strong>TypeScript</strong> service.

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">The server was rewritten in Rust. The TypeScript client never changed a line.</div>

<!--
We did a smaller version of this at BrowserOS. And the interesting part is not the Rust. It is that the TypeScript app talking to that server did not change at all.
-->

---
layout: default
class: neo-slide
---

<div class="neo-top">
  <span class="neo-logo"><span class="neo-b">BrowserOS</span> <span class="neo-neo">neo</span></span>
  <span class="neo-ph">browseros.com/neo</span>
</div>

<div class="grid grid-cols-2 gap-14 neo-main">
  <div>
    <div class="neo-eyebrow">Browser for AI agents</div>
    <h1 class="neo-h1">The Missing <em>Browser</em> for Claude, Cowork &amp; Codex.</h1>
    <p class="neo-body">A free, open-source browser your agents drive with your <strong>logged-in accounts</strong>. Import your Chrome logins in one click, connect the agent, and hand off the boring tabs.</p>
  </div>
  <div class="neo-feats">
    <div class="neo-feat"><div class="neo-flabel">Privacy-first</div><div class="neo-fval">Browsing data never leaves your machine.</div></div>
    <div class="neo-feat"><div class="neo-flabel">Chrome import</div><div class="neo-fval">One click. Bring your logins.</div></div>
    <div class="neo-feat"><div class="neo-flabel">Run many</div><div class="neo-fval">A fleet of agents at once.</div></div>
    <div class="neo-feat"><div class="neo-flabel">Replay</div><div class="neo-fval">See exactly what each one did.</div></div>
  </div>
</div>

<div class="neo-tie">Its local backend, <strong>claw-server</strong>, is the service we rewrote. That is case study one.</div>

<style>
.neo-slide.slidev-layout {
  background: #ffffff;
  background-image: none;
  font-family: 'Inter', ui-sans-serif, system-ui, sans-serif;
  color: #374151;
  padding: 2.6rem 3.4rem;
  letter-spacing: normal;
}
.neo-slide .neo-top { display: flex; align-items: baseline; justify-content: space-between; }
.neo-slide .neo-logo { font-size: 1.4rem; font-weight: 800; letter-spacing: -0.02em; }
.neo-slide .neo-b { color: #1d4ed8; }
.neo-slide .neo-neo { color: #1d4ed8; font-family: 'EB Garamond', Georgia, serif; font-style: italic; font-weight: 500; }
.neo-slide .neo-ph { font-size: 0.85rem; color: #9ca3af; }
.neo-slide .neo-main { align-items: start; margin-top: 1.8rem; }
.neo-slide .neo-eyebrow { font-size: 0.76rem; text-transform: uppercase; letter-spacing: 0.18em; color: #9ca3af; font-weight: 600; }
.neo-slide .neo-h1 { font-family: 'EB Garamond', Georgia, serif; font-weight: 400; font-size: 3rem; line-height: 1.08; color: #393f49; margin: 0.5rem 0 0 0; letter-spacing: 0; }
.neo-slide .neo-h1 em { color: #1d4ed8; font-style: italic; }
.neo-slide .neo-body { font-family: 'Inter', sans-serif; font-size: 1.12rem; color: #4b5563; line-height: 1.5; margin-top: 1.3rem; max-width: 34ch; }
.neo-slide .neo-body strong { color: #111827; font-weight: 700; }
.neo-slide .neo-feats { display: grid; grid-template-columns: 1fr 1fr; gap: 1.6rem 1.4rem; padding-top: 0.4rem; }
.neo-slide .neo-flabel { font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.12em; color: #9ca3af; font-weight: 700; }
.neo-slide .neo-fval { font-size: 1rem; color: #374151; margin-top: 0.35rem; line-height: 1.35; }
.neo-slide .neo-tie { margin-top: 2rem; font-size: 1rem; color: #6b7280; }
.neo-slide .neo-tie strong { color: #1d4ed8; font-weight: 600; }
</style>

<!--
Quick product context. The service we just talked about powers BrowserOS neo: a free, open-source browser that AI agents drive using your real, logged-in accounts. Import your Chrome logins, point Claude Code or Codex at it, run a fleet at once, and replay what they did. Its local backend is claw-server, which is exactly the TypeScript-to-Rust case study we are about to walk through.
-->

---
layout: statement
---

# How do you rewrite the server in Rust and never touch the <strong>TypeScript</strong> client?

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">This is a TypeScript talk. Not a Rust one.</div>

<!--
This is the question the whole talk answers. We changed the server's language. The TypeScript app on the other side did not change a line. How is that even possible? Everything after this slide is the answer.
-->

---

<div class="eyebrow">Why large refactors are scary</div>

## Big refactors break on two things.

<div class="grid grid-cols-2 gap-6" style="margin-top: 1.6rem;">
  <div class="card">
    <div class="card-k">01 · The seam</div>
    <div class="card-t">Type safety lives in the compiler.</div>
    <div class="card-b">Across a boundary it silently drifts, and you find out in production.</div>
  </div>
  <div class="card">
    <div class="card-k">02 · The volume</div>
    <div class="card-t">Thousands of correct edits.</div>
    <div class="card-b">Mechanical, tedious, and exactly where humans get bored and slip.</div>
  </div>
</div>

<div style="color: var(--color-steel); font-size: 1.2rem; margin-top: 1.6rem;">A first-class contract fixes the seam. AI agents clear the volume. Framework parity keeps the shape.</div>

---
layout: section
---

<div class="mono" style="color: var(--color-signal-orange); font-size: 5rem; font-weight: 700; line-height: 1;">01</div>

# Case study: <strong>claw-server</strong>, TypeScript to Rust

<div class="mono" style="color: var(--color-ash); margin-top: 1rem; font-size: 1rem;">packages/archive/claw-server-ts &nbsp;→&nbsp; apps/claw-server-rust</div>

---

<div class="eyebrow">Context</div>

## claw-server is the local backend for a browser that agents drive.

<div class="diagram">
  <div class="node">AI agent</div>
  <div class="arrow">→</div>
  <div class="node -spot">claw-server · MCP + REST</div>
  <div class="arrow">→</div>
  <div class="node">Chromium over CDP</div>
</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.6rem; max-width: 62ch;">
One process, two surfaces: a single MCP endpoint every agent connects to, and a typed REST API a desktop cockpit app reads. It ships inside BrowserOS.
</div>

<!--
claw-server runs on your machine. Agents talk to it over MCP to drive a real browser, and a desktop UI reads a REST API for session history. Both surfaces matter for the type-safety story.
-->

---

<div class="eyebrow">Why move at all</div>

## Three reasons to leave TypeScript, here.

<div class="grid grid-cols-3 gap-5" style="margin-top: 1.6rem;">
  <div class="card">
    <div class="card-k">Ship</div>
    <div class="card-t">One native binary.</div>
    <div class="card-b">It ships inside a desktop browser. A stripped Rust binary beats bundling a JS runtime.</div>
  </div>
  <div class="card">
    <div class="card-k">Stay up</div>
    <div class="card-t">Memory safety.</div>
    <div class="card-b">An always-on daemon streaming CDP frames. No GC jitter, no slow leaks.</div>
  </div>
  <div class="card">
    <div class="card-k">Fit in</div>
    <div class="card-t">Shared Rust crates.</div>
    <div class="card-b">Sits next to a native product and shares its CDP and MCP crates.</div>
  </div>
</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.5rem;">And AI made the rewrite cheap enough that those long-term wins were finally worth it.</div>

<!--
This is not a "TypeScript is slow" story. Three concrete reasons: it ships in a desktop app, it is an always-on daemon, and it lives next to a native C++ product. The reason it happened now is that AI dropped the cost of the rewrite.
-->

---

<Pill>Real · before</Pill>

## Act 1. Hono RPC: the server's type <em>is</em> the client's type

```ts {all|1-2|4|5-6}
// apps/claw-app/modules/api/client.ts
import type { AppType } from '@browseros/claw-server/server'
import { hc } from 'hono/client'

const client = hc<AppType>(baseUrl)
const res = await client.api.v1.sessions.$get()
//    ^ fully typed, inferred straight from the server's routes
```

<div class="caption">git show 9271e3cef^ · the pre-migration client</div>

<!--
Before the migration, the client used Hono RPC. You import AppType, which is literally typeof the server's routes, and hc gives you a fully typed client. No codegen. Gorgeous.
-->

---

<Pill spot>The idea</Pill>

## Beautiful types. Trapped in one language.

<div class="idea-list">

- `hc<AppType>()` infers the whole API from `typeof routes`
- Zero codegen, zero drift, and it all lives in the compiler
- <strong>But `typeof routes` cannot cross into Rust</strong>

</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.4rem;">A Rust server cannot export a TypeScript type. So the contract had to leave TypeScript first.</div>

<style>
.idea-list li { font-size: 1.4rem; line-height: 1.8; }
</style>

---
layout: statement
---

<div class="eyebrow" style="color: var(--color-ash)">Act 2 · the unlock</div>

# We moved the contract out of TypeScript. <strong>First.</strong>

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">Still a TypeScript server at this point. Only the contract changed.</div>

---

<Pill>Real · the contract</Pill>

## One hand-written OpenAPI document is the source of truth.

```yaml {all|2-5|7-10}
openapi: 3.0.3
info:
  title: BrowserClaw API
  description: Implementation-neutral REST contract
    shared by BrowserClaw clients and servers.
paths:
  /api/v1/sessions:
    $ref: ./paths/sessions.yaml#/collection
  /api/v1/cockpit/stats:
    $ref: ./paths/cockpit.yaml#/stats
```

<div class="caption">contracts/claw-api/openapi.yaml</div>

---

<Pill>Real · the pipeline</Pill>

## One spec. Three generated trees. Byte-for-byte.

```sh
$ bun run codegen:claw-api            # one spec in, three trees out
  → packages/claw-api/                TypeScript DTO models
  → packages/claw-api-client/         TypeScript operations client
  → crates/claw-api/                  Rust serde structs

$ bun run codegen:claw-api:check      # CI drift gate, byte-identical
```

<div class="caption">scripts/codegen/claw-api.ts · OpenAPI Generator, pinned + rustfmt-normalized in Docker</div>

<!--
One script reads the YAML and generates the TypeScript client and the Rust structs from the same spec. A check mode regenerates and compares byte-for-byte in CI. If anyone hand-edits generated code or forgets to regenerate, the build goes red.
-->

---

<Pill spot>The idea</Pill>

## The contract is the source of truth. Both sides are generated.

<div class="diagram">
  <div class="node -spot">openapi.yaml</div>
  <div class="arrow">→</div>
  <div class="branch">
    <div class="node">Rust serde DTOs &nbsp;·&nbsp; crates/claw-api</div>
    <div class="node">TS client types &nbsp;·&nbsp; packages/claw-api-client</div>
  </div>
</div>

<div style="color: var(--color-steel); font-size: 1.2rem; margin-top: 1.6rem;">Now the server's language is a free choice, and drift between the two sides is a build failure.</div>

---
layout: statement
---

<div class="eyebrow" style="color: var(--color-ash)">Act 3 · the port</div>

# Now the server can be anything. <strong>It became Rust.</strong>

<div class="mono" style="color: var(--color-ash); font-size: 1.05rem; margin-top: 1rem;">axum · sea-orm · rmcp &nbsp;·&nbsp; phased A to D · 98 commits</div>

---

<Pill>Real · the Rust server</Pill>

## Handlers return the generated contract type. Directly.

```rust {all|1|3-6|7}
use claw_api::models::CockpitStats;

pub async fn stats(
    State(state): State<AppState>,
) -> Result<Json<CockpitStats>, CanonicalError> {
    let aggregate = state.session_efficiency.aggregate().await?;
    Ok(Json(to_contract(aggregate)))
}
```

<div class="caption">apps/claw-server-rust/src/api/http/cockpit.rs · CockpitStats is generated from the same YAML the TS client uses</div>

---
layout: two-cols
layoutClass: gap-8
---

<Pill>Real · same shapes</Pill>

### Drizzle · TypeScript

```ts
export const toolDispatches = sqliteTable(
  'tool_dispatches',
  {
    id: integer('id').primaryKey(),
    agentId: text('agent_id').notNull(),
    toolName: text('tool_name').notNull(),
    durationMs: integer('duration_ms'),
  },
)

type Row = typeof toolDispatches.$inferSelect
```

::right::

<div style="height: 2.4rem"></div>

### sea-orm · Rust

```rust
#[derive(Clone, Debug, DeriveEntityModel)]
#[sea_orm(table_name = "tool_dispatches")]
pub struct Model {
    #[sea_orm(primary_key)]
    pub id: i64,
    pub agent_id: String,
    pub tool_name: String,
    pub duration_ms: Option<i64>,
}
```

<!--
We picked sea-orm specifically because it preserves the Drizzle style: one entity per file, typed columns, named migrations. Same shapes, different syntax.
-->

---

<Pill spot>The idea</Pill>

## Every TypeScript tool had a Rust twin.

<table class="parity">
  <thead><tr><th>Concern</th><th>TypeScript</th><th></th><th>Rust</th></tr></thead>
  <tbody>
    <tr><td class="why">ORM</td><td class="ts">Drizzle</td><td class="arrow-cell">→</td><td class="rust">sea-orm</td></tr>
    <tr><td class="why">Web framework</td><td class="ts">Hono</td><td class="arrow-cell">→</td><td class="rust">axum</td></tr>
    <tr><td class="why">MCP server</td><td class="ts">@modelcontextprotocol/sdk</td><td class="arrow-cell">→</td><td class="rust">rmcp</td></tr>
    <tr><td class="why">Schemas</td><td class="ts">Zod</td><td class="arrow-cell">→</td><td class="rust">serde + schemars</td></tr>
    <tr><td class="why">Lint + format</td><td class="ts">Biome</td><td class="arrow-cell">→</td><td class="rust">clippy + rustfmt</td></tr>
  </tbody>
</table>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.4rem;">A language rewrite did not have to be an architecture rewrite. That is what made the port mechanical.</div>

---

<div class="eyebrow">A small trick</div>

## We did not migrate the database. We adopted it.

<div class="diagram">
  <div class="node -ghost">Drizzle-owned SQLite</div>
  <div class="arrow">→</div>
  <div class="node">first sea-orm migration</div>
  <div class="arrow">→</div>
  <div class="node -spot">DROP __drizzle_migrations</div>
</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.6rem; max-width: 62ch;">
The Rust server opened the same SQLite file, rebuilt the schema, and dropped the old migration ledger. Existing rows preserved. Zero data migration.
</div>

---
layout: statement
---

# How did we know the port was correct?

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">Not vibes. Not "the tests pass." Something stronger.</div>

---

<div class="eyebrow">Proof, not vibes</div>

## We ran both servers and compared them.

<div style="margin-top: 1.6rem;">
<StatCard :items="[
  { n: '205/205', l: 'cross-server tests passing' },
  { n: '37', l: 'agent review · 10 findings fixed' },
  { n: '98', l: 'commits, all test-gated' },
]" />
</div>

<div class="spotlight" style="margin-top: 1.8rem; max-width: 64ch;">
The differential harness compared the TypeScript and Rust servers semantically, and wrote the migration to-do list itself.
</div>

---

<Pill spot>The idea</Pill>

## Keep the old code as the oracle. Let the difference be your to-do list.

<div class="diagram">
  <div class="branch">
    <div class="node">TS server</div>
    <div class="node">Rust server</div>
  </div>
  <div class="arrow">→</div>
  <div class="node">same real browser, same calls</div>
  <div class="arrow">→</div>
  <div class="node -spot">compare · divergence ledger</div>
</div>

<div style="color: var(--color-steel); font-size: 1.1rem; margin-top: 1.5rem; max-width: 64ch;">
The ledger fails only on a <em>new</em> disagreement. It caught a serde bug that silently zeroed the Rust node IDs, which no lower test tier could see.
</div>

<!--
This is the same move Bun made with its TypeScript test suite. Run the old and new side by side, compare behavior, and treat every new disagreement as a task. The harness literally generated the migration backlog.
-->

---

<Pill>From the pull request</Pill>

## And then we let a swarm of agents attack it.

<div class="quote">
"2-round verifier pass on the MCP rewrite, plus a 37-agent workflow review. All 10 confirmed findings fixed on-branch."
</div>
<div class="quote-src">PR #1839 · claw stack parity</div>

<div style="color: var(--color-steel); font-size: 1.1rem; margin-top: 1.6rem; max-width: 60ch;">
The shipped version is a 7-agent review, tiered by model, with a validation pass that drops false positives. Reviewers see only the diff and are told to assume it is wrong.
</div>

---
layout: statement
---

# Drift is a build failure. The language is a detail.

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">The server was rewritten in Rust. The TypeScript client never changed a line.</div>

---
layout: section
---

<div class="mono" style="color: var(--color-signal-orange); font-size: 5rem; font-weight: 700; line-height: 1;">02</div>

# Case study: <strong>agent-terminal</strong>, the other direction

<div class="mono" style="color: var(--color-ash); margin-top: 1rem; font-size: 1rem;">Rust &nbsp;→&nbsp; TypeScript &nbsp;·&nbsp; a terminal on your desktop, a companion on your phone</div>

---

<div class="eyebrow">The comms layer</div>

## A Tauri desktop app and an Expo phone, talking directly.

<div class="diagram">
  <div class="node">Expo phone (TS)</div>
  <div class="arrow">⇄</div>
  <div class="node -spot">TLS WebSocket · LAN</div>
  <div class="arrow">⇄</div>
  <div class="node">Tauri desktop (Rust)</div>
</div>

<div class="caption">mDNS discovery + QR cert-pinning · no cloud relay</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.6rem; max-width: 62ch;">
Here the protocol lives in Rust. So the source of truth is Rust, and TypeScript is what gets generated. The mirror image of case study 1.
</div>

---

<Pill>Real · one source of truth</Pill>

## The wire protocol is a Rust enum.

```rust {all|1|4|5-11}
#[typeshare]
#[derive(Serialize, Deserialize)]
#[serde(tag = "op", content = "body", rename_all = "snake_case")]
pub enum ClientFrame {
    Auth { token: String },
    Subscribe { tab_id: String, scrollback: u32 },
    Resume {
        tab_id: String,
        #[typeshare(serialized_as = "u32")]
        last_seq: u64,
    },
}
```

<div class="caption">src-tauri/src/protocol.rs</div>

---

<Pill>Real · generated</Pill>

## TypeScript, generated straight from it.

```ts {all|1|2-4}
// apps/companion/src/modules/wss/protocol.gen.ts  (generated by typeshare)
export type ClientFrame =
  | { op: 'auth';      body: { token: string } }
  | { op: 'subscribe'; body: { tab_id: string; scrollback: number } }
  | { op: 'resume';    body: { tab_id: string; last_seq: number } }
```

<div class="caption">A serde adjacently-tagged enum becomes a TypeScript discriminated union</div>

---

<Pill spot>The idea</Pill>

## Rust is the truth. TypeScript is generated. Drift is a compile error.

<div class="diagram">
  <div class="node -spot">protocol.rs</div>
  <div class="arrow">→</div>
  <div class="node">cargo xtask regen-protocol</div>
  <div class="arrow">→</div>
  <div class="node">protocol.gen.ts</div>
</div>

<div class="mono" style="color: var(--color-steel); font-size: 0.95rem; margin-top: 1.4rem;">CI gate: &nbsp; cargo xtask regen-protocol && git diff --exit-code</div>

<div style="color: var(--color-steel); font-size: 1.1rem; margin-top: 1.2rem;">The phone consumes the union type-only, zero casts. A drifted field is a red build, not a 2am reconnect loop.</div>

---

<div class="eyebrow">Being honest about it</div>

## Not everything is codegen. On purpose.

<div class="grid grid-cols-2 gap-6" style="margin-top: 1.4rem;">
  <div class="card">
    <div class="card-k">Codegen</div>
    <div class="card-t">The streaming protocol</div>
    <div class="card-b">High-frequency, always changing. Generated and drift-gated.</div>
  </div>
  <div class="card">
    <div class="card-k">Hand-mirrored</div>
    <div class="card-t">The pairing handshake</div>
    <div class="card-b">Small, stable, once per device. Copied by hand, with a "future typeshare sweep" note in the code.</div>
  </div>
</div>

<div style="color: var(--color-steel); font-size: 1.15rem; margin-top: 1.5rem;">Codegen has a cost. Draw the line where the surface actually changes.</div>

---

<div class="eyebrow">Two war stories</div>

## The seams where Rust and JavaScript disagree.

<div class="grid grid-cols-2 gap-6" style="margin-top: 1.4rem;">
  <div class="card">
    <div class="card-k">u64 → number</div>
    <div class="card-t">JavaScript has 53 bits.</div>
    <div class="card-b"><code>#[typeshare(serialized_as = "u32")]</code> keeps Rust <code>u64</code> on the wire but emits TS <code>number</code>.</div>
  </div>
  <div class="card">
    <div class="card-k">omit vs null</div>
    <div class="card-t">Optional means absent.</div>
    <div class="card-b"><code>skip_serializing_if</code> keeps the generated <code>field?: string</code> contract honest, and tests pin the exact JSON.</div>
  </div>
</div>

<!--
Two real seams where Rust and JavaScript disagree, both straight from agent-terminal's protocol.rs.

u64 to number. JavaScript numbers are floats, so they only safely hold integers up to 2 to the 53rd. Rust's u64 is 64 bits. typeshare actually refuses to emit a raw u64, because it cannot map it safely. The escape hatch is `#[typeshare(serialized_as = "u32")]`: it tells typeshare to generate the TypeScript type as if the field were a u32, so TS sees `number`, while the Rust value and the JSON on the wire stay a real u64. That is how the protocol's seq and last_seq counters keep full precision without lying to TypeScript.

omit vs null. In serde, an Option field marked `skip_serializing_if = "Option::is_none"` is omitted from the JSON when it is None: the key is simply not there. typeshare generates that as `field?: string` in TypeScript, meaning a string or absent. If serde instead sent `"field": null`, it would break that generated optional contract. So skip_serializing_if keeps the wire shape matching the generated optional field, and tests pin the exact JSON so it can never drift.
-->

---
layout: statement
---

# Four ways to move a type across a language line.

---

<div class="eyebrow">The spectrum</div>

## Pick your source of truth.

<div class="grid grid-cols-2 gap-5" style="margin-top: 1.4rem;">
  <div class="card">
    <div class="card-k">Compiler-inferred</div>
    <div class="card-t">Hono RPC</div>
    <div class="card-b">Zero codegen, magical. One language only.</div>
  </div>
  <div class="card">
    <div class="card-k">Spec-first</div>
    <div class="card-t">OpenAPI</div>
    <div class="card-b">The contract is the truth. Generates every side.</div>
  </div>
  <div class="card">
    <div class="card-k">Code-first</div>
    <div class="card-t">typeshare</div>
    <div class="card-b">Rust is the truth. TypeScript is generated.</div>
  </div>
  <div class="card">
    <div class="card-k">Manual</div>
    <div class="card-t">Hand-mirrored</div>
    <div class="card-b">For the small and stable. Cheapest until it is not.</div>
  </div>
</div>

---
layout: statement
---

# One source of truth. Generate the other side. <strong>Gate drift in CI.</strong>

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">The agents do the volume. The contract keeps them honest.</div>

---
layout: section
---

<div class="mono" style="color: var(--color-signal-orange); font-size: 4rem; font-weight: 700; line-height: 1;">↺</div>

# The playbook. Bun's, and ours. <strong>Steal it.</strong>

---

<div class="eyebrow">The reusable workflow</div>

## Six moves that make a large refactor an agent can do.

<div class="grid grid-cols-3 gap-4 wf" style="margin-top: 1.4rem;">
  <div class="card"><div class="card-k">01</div><div class="card-t">A patterns map</div><div class="card-b">Bun's PORTING.md, our parity table. Make the port mechanical.</div></div>
  <div class="card"><div class="card-k">02</div><div class="card-t">Plan-first, tiny tasks</div><div class="card-b">Failing test, minimal code, pass, commit.</div></div>
  <div class="card"><div class="card-k">03</div><div class="card-t">A neutral oracle</div><div class="card-b">A test suite or harness that writes the to-do list.</div></div>
  <div class="card"><div class="card-k">04</div><div class="card-t">Contract-first codegen</div><div class="card-b">Drift becomes a build failure.</div></div>
  <div class="card"><div class="card-k">05</div><div class="card-t">Adversarial review</div><div class="card-b">Reviewer sees only the diff, assumes it is wrong.</div></div>
  <div class="card"><div class="card-k">06</div><div class="card-t">Parallelism + a gate</div><div class="card-b">Worktrees, and a fixed verify loop per PR.</div></div>
</div>

<style>
.wf .card { padding: 0.9rem 1rem; }
.wf .card-t { font-size: 1.05rem; margin-bottom: 0.3rem; }
.wf .card-b { font-size: 0.9rem; line-height: 1.32; }
</style>

---
layout: statement
---

# The compiler and the tests are the work queue. <strong>The agents clear it.</strong>

---
layout: statement
---

# The client never changed while the server changed languages.

<div style="color: var(--color-ash); font-size: 1.3rem; margin-top: 1rem;">Because the contract was the real source of truth.</div>

---
layout: two-cols
layoutClass: gap-12
---

<div class="eyebrow">Thank you</div>

<h1 style="font-size: 2.3rem; font-weight: 700; letter-spacing: -0.02em; line-height: 1.12; max-width: 20ch; margin: 0.4rem 0 0 0;">TypeScript's superpower is not <code>hc&lt;AppType&gt;</code>. It is a <strong>contract</strong> that outlives the language.</h1>

<div style="color: var(--color-steel); font-size: 1.1rem; margin-top: 1.4rem; max-width: 32ch;">Questions, war stories, counter-takes. The slides are public.</div>

::right::

<div class="contacts">
  <div class="crow"><span class="clabel">GitHub</span><span class="cval">@DaniAkash</span></div>
  <div class="crow"><span class="clabel">X</span><span class="cval">@dani_akash_</span></div>
  <div class="crow"><span class="clabel">Web</span><span class="cval">daniakash.com</span></div>
  <div class="crow"><span class="clabel">Slides</span><span class="cval" style="font-size: 1.05rem;">github.com/DaniAkash/ai-workflows-for-ts-refactors</span></div>
</div>

<style>
.contacts { display: flex; flex-direction: column; margin-top: 1rem; }
.crow { display: flex; flex-direction: column; gap: 0.2rem; padding: 0.9rem 0; border-top: 1px solid rgba(0,0,0,0.14); }
.crow:last-child { border-bottom: 1px solid rgba(0,0,0,0.14); }
.clabel { font-family: var(--font-geist-mono); font-size: 0.72rem; text-transform: uppercase; letter-spacing: 0.2em; color: var(--color-steel); }
.cval { font-weight: 600; font-size: 1.5rem; letter-spacing: -0.01em; }
</style>

<!--
Questions. If you take one thing home: make the contract a real artifact, generate both sides, and let the agents do the rest.
-->
