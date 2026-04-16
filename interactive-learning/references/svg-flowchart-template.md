# SVG Flowchart Template

Use this reference when the task is a flowchart, note workflow diagram, or a reusable SVG-diagram generation pattern.

## Default Position

- Prefer custom `SVG` as the default output for flowcharts
- In Obsidian notes, prefer inline `SVG` inside `dataviewjs` when container width adaptation matters
- Do not choose Mermaid for flowcharts unless the user explicitly asks for Mermaid syntax

## Template Goal

The template should produce diagrams that are:

- visually polished rather than tool-like
- stable under host-container scaling
- readable with Chinese labels
- easy to adapt into wide and narrow variants

## Layout Tokens

Start from a small set of repeated spacing tokens instead of ad hoc coordinates.

- outer padding: `32`
- section gap: `28`
- node gap horizontal: `28`
- node gap vertical: `24`
- node min height: `72`
- node corner radius: `18`
- title font size: `24`
- primary text font size: `22`
- secondary text font size: `14`
- connector stroke width: `2.5`

These are defaults, not hard rules. Increase spacing before shrinking text.

## Node System

Use a card-based node system:

- Rounded rectangle as the main body
- Primary label centered on the first line
- Optional secondary label centered on the second line
- Muted fill and medium-contrast border
- Reserve at least `20` horizontal padding and `16` vertical padding inside a node

Suggested node classes:

- start/end node: slightly stronger fill
- process node: standard rounded card
- decision node: softened diamond only when an actual branch must be emphasized
- note/annotation node: lighter dashed border

## Connector System

Prefer connectors that feel designed, not mechanical.

- Use solid connectors for the main path
- Use dashed connectors only for optional, side-note, or feedback paths
- Prefer cubic or quadratic curves for cross-lane routing
- Prefer short rounded elbows only when the path is mostly orthogonal
- Keep arrowheads modest and consistent
- Leave explicit connector gutters between node columns and rows

Recommended curve shapes:

- vertical progression: mostly straight line with a little breathing room near arrowheads
- side transition: cubic bezier with one horizontal sweep
- feedback loop: larger outer curve with dashed stroke

## Wide vs Narrow Strategy

Do not squeeze one layout forever.

- Wide layout: left-to-right sections or a two-column composition
- Narrow layout: stacked vertical composition with the same node style
- If the note width is uncertain, prepare both variants conceptually
- In `dataviewjs`, swap between wide and narrow variants based on wrapper width if needed

## SVG Skeleton

Use this as a starting point and adapt coordinates to the content.

```svg
<svg
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 960 560"
  width="960"
  height="560"
  preserveAspectRatio="xMidYMin meet"
  role="img"
  aria-label="流程图">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#5b6b8c" />
    </marker>
    <style>
      .title { font-size: 24px; font-weight: 700; fill: #24324a; }
      .label { font-size: 22px; font-weight: 600; fill: #24324a; }
      .meta { font-size: 14px; fill: #5f6f86; }
      .node { fill: #f6f8fc; stroke: #c8d2e6; stroke-width: 1.5; }
      .node-strong { fill: #eaf1ff; stroke: #9db4ea; stroke-width: 1.5; }
      .node-note { fill: #f8fafc; stroke: #c7d0dc; stroke-width: 1.5; stroke-dasharray: 6 5; }
      .line { fill: none; stroke: #5b6b8c; stroke-width: 2.5; marker-end: url(#arrow); }
      .line-dashed { fill: none; stroke: #7d8798; stroke-width: 2.2; stroke-dasharray: 7 6; marker-end: url(#arrow); }
    </style>
  </defs>

  <text x="48" y="44" class="title">示例流程图</text>

  <rect x="80" y="96" width="220" height="84" rx="18" class="node-strong" />
  <text x="190" y="132" class="label" text-anchor="middle">
    <tspan x="190" dy="0">开始</tspan>
    <tspan x="190" dy="24" class="meta">触发条件</tspan>
  </text>

  <rect x="370" y="96" width="240" height="84" rx="18" class="node" />
  <text x="490" y="132" class="label" text-anchor="middle">
    <tspan x="490" dy="0">处理步骤</tspan>
    <tspan x="490" dy="24" class="meta">核心动作</tspan>
  </text>

  <rect x="680" y="96" width="200" height="84" rx="18" class="node" />
  <text x="780" y="132" class="label" text-anchor="middle">
    <tspan x="780" dy="0">结果</tspan>
    <tspan x="780" dy="24" class="meta">输出状态</tspan>
  </text>

  <path d="M 300 138 C 326 138, 338 138, 370 138" class="line" />
  <path d="M 610 138 C 636 138, 648 138, 680 138" class="line" />

  <rect x="370" y="248" width="240" height="84" rx="18" class="node-note" />
  <text x="490" y="284" class="label" text-anchor="middle">
    <tspan x="490" dy="0">补充说明</tspan>
    <tspan x="490" dy="24" class="meta">可选分支 / 注意点</tspan>
  </text>

  <path d="M 490 180 C 490 208, 490 214, 490 248" class="line-dashed" />
</svg>
```

## DataviewJS Wrapper Pattern

Use this wrapper when the diagram lives inside an Obsidian note.

```dataviewjs
const root = dv.el("div", "", { cls: "il-flowchart-wrap" });
root.innerHTML = `
  <div style="width:100%; overflow-x:auto; padding: 4px 0;">
    <!-- inline svg here -->
  </div>
`;
```

Apply these outer rules:

- `svg { display:block; width:100%; height:auto; }`
- keep a stable `viewBox`
- let the wrapper manage note-width adaptation

## Authoring Checklist

Before finalizing, check:

1. The reading direction is obvious within two seconds
2. Every arrow endpoint lands in reserved whitespace, not in text
3. Node widths match their label length with enough padding
4. Wide labels wrap into a second line before the node becomes cramped
5. The diagram still reads clearly when scaled down in an Obsidian note
6. If the wide version feels tight, switch to a narrow stacked version instead of compressing it further

## Preferred Output Heuristic

- For a plain standalone diagram: output a single polished `SVG`
- For an Obsidian note flowchart: output `dataviewjs` with inline `SVG`
- For a responsive note component: prepare a wide and narrow composition, then let the wrapper choose between them
