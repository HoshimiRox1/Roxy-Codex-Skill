# 交互式知识点 DataviewJS 模板

下面这份文件把常见的三类 Obsidian 交互式知识点收成可复用模板：

- 折叠路线图
- 标签页知识点卡组
- 步骤式解释器

每个模板都遵循同一个原则：先改顶部数据，再改文案，最后才动样式。

---

## 1. 折叠路线图模板

```dataviewjs
(() => {
  const phases = [
    {
      badge: "阶段 0",
      title: "准备阶段",
      meta: "1-2 天",
      summary: "先建立基本认知和运行环境。",
      items: [
        { text: "完成安装和最小可运行环境", tag: "运维" },
        { text: "跑通第一组核心命令", tag: "命令" },
        { text: "建立最基础的心智模型", tag: "概念" }
      ],
      open: true
    },
    {
      badge: "阶段 1",
      title: "核心知识点",
      meta: "3-5 天",
      summary: "掌握日常使用最频繁的部分。",
      items: [
        { text: "理解核心结构或对象", tag: "概念" },
        { text: "掌握关键命令或接口", tag: "命令" },
        { text: "能结合代码或场景使用", tag: "代码" }
      ]
    },
    {
      badge: "阶段 2",
      title: "生产问题",
      meta: "持续迭代",
      summary: "开始处理稳定性、性能和恢复问题。",
      items: [
        { text: "识别典型故障与恢复路径", tag: "运维" },
        { text: "建立监控与调优思路", tag: "架构" }
      ]
    }
  ];

  const root = dv.el("div", "");
  root.className = "il-roadmap";

  root.innerHTML = `
    <style>
      .il-roadmap {
        --il-border: var(--background-modifier-border);
        --il-text: var(--text-normal);
        --il-muted: var(--text-muted);
        --il-faint: var(--text-faint);
        font-family: var(--font-sans, sans-serif);
        margin: 1rem 0;
      }
      .il-roadmap-phase {
        border: 1px solid var(--il-border);
        border-radius: 14px;
        overflow: hidden;
        margin-bottom: 14px;
        background: color-mix(in srgb, var(--background-primary) 96%, #7f8ea3 4%);
      }
      .il-roadmap-head {
        padding: 12px 14px;
        display: flex;
        gap: 10px;
        align-items: center;
        cursor: pointer;
      }
      .il-roadmap-badge {
        font-size: 11px;
        font-weight: 700;
        border-radius: 999px;
        padding: 4px 9px;
        background: color-mix(in srgb, var(--interactive-accent) 16%, transparent);
        color: var(--interactive-accent);
      }
      .il-roadmap-title {
        flex: 1;
        font-size: 14px;
        font-weight: 700;
        color: var(--il-text);
      }
      .il-roadmap-meta {
        font-size: 12px;
        color: var(--il-muted);
      }
      .il-roadmap-chevron {
        font-size: 12px;
        color: var(--il-faint);
        transition: transform .18s ease;
      }
      .il-roadmap-chevron.open {
        transform: rotate(90deg);
      }
      .il-roadmap-body {
        display: none;
        padding: 0 14px 14px;
      }
      .il-roadmap-body.open {
        display: block;
      }
      .il-roadmap-summary {
        margin: 4px 0 10px;
        color: var(--il-muted);
        font-size: 13px;
        line-height: 1.7;
      }
      .il-roadmap-list {
        list-style: none;
        margin: 0;
        padding: 0;
      }
      .il-roadmap-item {
        display: flex;
        gap: 10px;
        align-items: flex-start;
        padding: 7px 0;
      }
      .il-roadmap-dot {
        width: 6px;
        height: 6px;
        border-radius: 50%;
        margin-top: 7px;
        background: var(--interactive-accent);
        flex-shrink: 0;
      }
      .il-roadmap-item-text {
        flex: 1;
        font-size: 13px;
        line-height: 1.6;
        color: var(--il-text);
      }
      .il-roadmap-tag {
        font-size: 11px;
        color: var(--il-muted);
        border: 1px solid var(--il-border);
        border-radius: 999px;
        padding: 2px 7px;
        white-space: nowrap;
      }
    </style>
  `;

  phases.forEach((phase) => {
    const node = document.createElement("section");
    node.className = "il-roadmap-phase";
    node.innerHTML = `
      <div class="il-roadmap-head">
        <span class="il-roadmap-badge">${phase.badge}</span>
        <span class="il-roadmap-title">${phase.title}</span>
        <span class="il-roadmap-meta">${phase.meta}</span>
        <span class="il-roadmap-chevron ${phase.open ? "open" : ""}">▶</span>
      </div>
      <div class="il-roadmap-body ${phase.open ? "open" : ""}">
        <div class="il-roadmap-summary">${phase.summary}</div>
        <ul class="il-roadmap-list">
          ${phase.items.map((item) => `
            <li class="il-roadmap-item">
              <span class="il-roadmap-dot"></span>
              <span class="il-roadmap-item-text">${item.text}</span>
              <span class="il-roadmap-tag">${item.tag}</span>
            </li>
          `).join("")}
        </ul>
      </div>
    `;

    const head = node.querySelector(".il-roadmap-head");
    const body = node.querySelector(".il-roadmap-body");
    const chevron = node.querySelector(".il-roadmap-chevron");
    head.addEventListener("click", () => {
      const isOpen = body.classList.toggle("open");
      chevron.classList.toggle("open", isOpen);
    });

    root.appendChild(node);
  });
})();
```

---

## 2. 标签页知识点卡组模板

```dataviewjs
(() => {
  const topics = [
    {
      label: "概念 A",
      color: "#2E5B9A",
      bg: "#EAF1FB",
      visual: `
        <div style="font-size:12px;color:var(--text-faint);margin-bottom:8px">这里放这个知识点的微型结构图或示意</div>
        <div style="padding:10px 12px;border-radius:12px;background:#EAF1FB;color:#173C6A;border:1px solid #C8DAF2">key → value / 对象结构 / 小图示</div>
      `,
      usecase: "一句话说明它最适合解决什么问题。",
      cmds: [
        ["CMD_A", "命令或接口的作用", "CLI / shell 示例", "Python / JS 示例"],
        ["CMD_B", "第二个常用命令或接口", "CLI / shell 示例", "Python / JS 示例"]
      ],
      note: "补一句容易混淆但很重要的提醒。"
    },
    {
      label: "概念 B",
      color: "#6E4AA3",
      bg: "#F1ECFA",
      visual: `
        <div style="font-size:12px;color:var(--text-faint);margin-bottom:8px">切换后看到另一组结构或流程</div>
        <div style="padding:10px 12px;border-radius:12px;background:#F1ECFA;color:#4A3072;border:1px solid #DBCDF1">另一个结构示意</div>
      `,
      usecase: "一句话说明 B 和 A 的核心区别。",
      cmds: [
        ["CMD_X", "做什么", "CLI / shell 示例", "Python / JS 示例"],
        ["CMD_Y", "做什么", "CLI / shell 示例", "Python / JS 示例"]
      ],
      note: "如果只记一个差异，就记这个。"
    }
  ];

  const root = dv.el("div", "");
  root.className = "il-tabs";
  root.innerHTML = `
    <style>
      .il-tabs {
        --il-border: var(--background-modifier-border);
        --il-text: var(--text-normal);
        --il-muted: var(--text-muted);
        margin: 1rem 0;
        font-family: var(--font-sans, sans-serif);
      }
      .il-tabs-row {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        margin-bottom: 12px;
      }
      .il-tab {
        border: 1px solid var(--il-border);
        background: transparent;
        color: var(--il-text);
        border-radius: 999px;
        padding: 7px 12px;
        cursor: pointer;
        font-size: 12px;
        font-weight: 700;
      }
      .il-panel {
        display: none;
        border: 1px solid var(--il-border);
        border-radius: 16px;
        padding: 14px;
        background: color-mix(in srgb, var(--background-primary) 96%, #7f8ea3 4%);
      }
      .il-panel.active {
        display: block;
      }
      .il-vis {
        margin-bottom: 12px;
      }
      .il-usecase {
        font-size: 13px;
        line-height: 1.7;
        color: var(--il-text);
        margin-bottom: 12px;
      }
      .il-cmd-list {
        list-style: none;
        margin: 0;
        padding: 0;
      }
      .il-cmd-item {
        border-top: 1px solid var(--il-border);
        padding: 10px 0;
      }
      .il-cmd-main {
        font-size: 13px;
        color: var(--il-text);
      }
      .il-cmd-toggle {
        display: inline-flex;
        align-items: center;
        padding: 2px 8px;
        border-radius: 999px;
        font-size: 11px;
        font-weight: 700;
        cursor: pointer;
      }
      .il-cmd-example {
        display: none;
        margin-top: 8px;
        font-size: 12px;
        line-height: 1.7;
        color: var(--il-muted);
      }
      .il-cmd-item.open .il-cmd-example {
        display: block;
      }
      .il-note {
        margin-top: 10px;
        padding: 8px 12px;
        border-left: 2px solid var(--il-border);
        color: var(--il-muted);
        font-size: 12.5px;
      }
    </style>
    <div class="il-tabs-row"></div>
    <div class="il-panels"></div>
  `;

  const tabsRow = root.querySelector(".il-tabs-row");
  const panels = root.querySelector(".il-panels");

  function activate(index) {
    tabsRow.querySelectorAll(".il-tab").forEach((tab, i) => {
      const active = i === index;
      const topic = topics[i];
      tab.style.background = active ? topic.color : "";
      tab.style.borderColor = active ? topic.color : "";
      tab.style.color = active ? "#fff" : "";
    });
    panels.querySelectorAll(".il-panel").forEach((panel, i) => {
      panel.classList.toggle("active", i === index);
    });
  }

  topics.forEach((topic, index) => {
    const tab = document.createElement("button");
    tab.className = "il-tab";
    tab.textContent = topic.label;
    tab.addEventListener("click", () => activate(index));
    tabsRow.appendChild(tab);

    const panel = document.createElement("section");
    panel.className = "il-panel";
    panel.innerHTML = `
      <div class="il-vis">${topic.visual}</div>
      <div class="il-usecase">${topic.usecase}</div>
      <ul class="il-cmd-list">
        ${topic.cmds.map(([name, desc, cli, code]) => `
          <li class="il-cmd-item">
            <div class="il-cmd-main">
              <span class="il-cmd-toggle" tabindex="0" role="button" style="background:${topic.bg};color:${topic.color}">${name}</span>
              ：${desc}
            </div>
            <div class="il-cmd-example">
              <div><strong>CLI</strong>：${cli}</div>
              <div><strong>代码</strong>：${code}</div>
            </div>
          </li>
        `).join("")}
      </ul>
      <div class="il-note">${topic.note}</div>
    `;
    panels.appendChild(panel);
  });

  root.addEventListener("click", (event) => {
    const toggle = event.target.closest(".il-cmd-toggle");
    if (!toggle) return;
    const item = toggle.closest(".il-cmd-item");
    if (!item) return;
    item.classList.toggle("open");
  });

  root.addEventListener("keydown", (event) => {
    const toggle = event.target.closest(".il-cmd-toggle");
    if (!toggle) return;
    if (event.key !== "Enter" && event.key !== " ") return;
    event.preventDefault();
    const item = toggle.closest(".il-cmd-item");
    if (!item) return;
    item.classList.toggle("open");
  });

  activate(0);
})();
```

---

## 3. 步骤式解释器模板

```dataviewjs
(() => {
  const steps = [
    {
      label: "1. 起点",
      title: "先说明起始状态",
      diagram: [
        ["输入", "accent"],
        ["处理", "main"],
        ["结果", "accent"]
      ],
      notes: [
        "第一步说明这个过程从哪里开始。",
        "强调这一阶段最关键的动作或条件。"
      ],
      takeaway: "一句话抓重点：先有输入，再进入主处理。"
    },
    {
      label: "2. 中间过程",
      title: "中间过程如何推进",
      diagram: [
        ["状态 A", "main"],
        ["转换", "accent"],
        ["状态 B", "main"]
      ],
      notes: [
        "这里解释过程中的状态变化。",
        "如果有误区或容易混淆的点，也放在这里。"
      ],
      takeaway: "一句话抓重点：中间过程的关键是状态转换。"
    },
    {
      label: "3. 收尾",
      title: "最终如何落地或恢复",
      diagram: [
        ["当前状态", "main"],
        ["收尾动作", "accent"],
        ["最终状态", "main"]
      ],
      notes: [
        "解释最后一步是如何结束的。",
        "说明这一步的结果为什么重要。"
      ],
      takeaway: "一句话抓重点：最终状态决定你如何理解整个过程。"
    }
  ];

  const root = dv.el("div", "");
  root.className = "il-stepper";
  root.innerHTML = `
    <style>
      .il-stepper {
        --il-border: var(--background-modifier-border);
        --il-text: var(--text-normal);
        --il-muted: var(--text-muted);
        --il-accent: var(--interactive-accent);
        font-family: var(--font-sans, sans-serif);
        border: 1px solid var(--il-border);
        border-radius: 16px;
        padding: 16px;
        margin: 1rem 0;
        background: color-mix(in srgb, var(--background-primary) 96%, #7f8ea3 4%);
      }
      .il-stepper-title {
        font-size: 15px;
        font-weight: 700;
        color: var(--il-text);
        margin-bottom: 6px;
      }
      .il-stepper-sub {
        font-size: 12.5px;
        line-height: 1.7;
        color: var(--il-muted);
        margin-bottom: 12px;
      }
      .il-stepper-steps {
        display: flex;
        gap: 8px;
        flex-wrap: wrap;
        margin-bottom: 12px;
      }
      .il-stepper-btn {
        border: 1px solid var(--il-border);
        background: transparent;
        color: var(--il-text);
        border-radius: 999px;
        padding: 6px 12px;
        font-size: 12px;
        cursor: pointer;
      }
      .il-stepper-btn.active {
        background: color-mix(in srgb, var(--il-accent) 16%, transparent);
        border-color: color-mix(in srgb, var(--il-accent) 45%, var(--il-border));
      }
      .il-stepper-panel {
        border: 1px solid var(--il-border);
        border-radius: 14px;
        padding: 14px;
        background: color-mix(in srgb, var(--background-primary) 98%, #6b7a8f 2%);
      }
      .il-stepper-scene {
        display: grid;
        grid-template-columns: 1.1fr 1fr;
        gap: 14px;
      }
      .il-stepper-diagram {
        border: 1px dashed var(--il-border);
        border-radius: 12px;
        padding: 12px;
      }
      .il-stepper-diagram-title {
        font-size: 13px;
        font-weight: 700;
        margin-bottom: 10px;
        color: var(--il-text);
      }
      .il-stepper-flow {
        display: flex;
        gap: 8px;
        flex-wrap: wrap;
        align-items: center;
      }
      .il-stepper-node {
        border-radius: 10px;
        padding: 8px 12px;
        font-size: 12.5px;
        font-weight: 700;
        border: 1px solid transparent;
      }
      .il-stepper-node.main {
        background: color-mix(in srgb, var(--background-primary) 84%, #7ca7d5 16%);
      }
      .il-stepper-node.accent {
        background: color-mix(in srgb, var(--background-primary) 84%, #8ec8aa 16%);
      }
      .il-stepper-arrow {
        color: var(--il-muted);
      }
      .il-stepper-notes h4 {
        margin: 0 0 8px;
        font-size: 13px;
        color: var(--il-text);
      }
      .il-stepper-notes ul {
        margin: 0;
        padding-left: 18px;
      }
      .il-stepper-notes li {
        margin-bottom: 8px;
        font-size: 12.8px;
        line-height: 1.65;
        color: var(--il-text);
      }
      .il-stepper-takeaway {
        margin-top: 12px;
        padding: 10px 12px;
        border-left: 3px solid var(--il-accent);
        color: var(--il-text);
        font-size: 12.8px;
        line-height: 1.7;
        background: color-mix(in srgb, var(--background-primary) 94%, #8ec8aa 6%);
      }
      @media (max-width: 860px) {
        .il-stepper-scene {
          grid-template-columns: 1fr;
        }
      }
    </style>
    <div class="il-stepper-title">步骤式解释器模板</div>
    <div class="il-stepper-sub">适合解释一个过程如何随时间或阶段展开，而不是做横向比较。</div>
    <div class="il-stepper-steps"></div>
    <div class="il-stepper-panel"></div>
  `;

  const stepsEl = root.querySelector(".il-stepper-steps");
  const panelEl = root.querySelector(".il-stepper-panel");

  function renderStep(index) {
    const step = steps[index];
    panelEl.innerHTML = `
      <div class="il-stepper-scene">
        <div class="il-stepper-diagram">
          <div class="il-stepper-diagram-title">${step.title}</div>
          <div class="il-stepper-flow">
            ${step.diagram.map(([text, type], i) => `
              <span class="il-stepper-node ${type}">${text}</span>
              ${i < step.diagram.length - 1 ? '<span class="il-stepper-arrow">→</span>' : ''}
            `).join("")}
          </div>
        </div>
        <div class="il-stepper-notes">
          <h4>这一阶段在说明什么</h4>
          <ul>${step.notes.map((note) => `<li>${note}</li>`).join("")}</ul>
        </div>
      </div>
      <div class="il-stepper-takeaway"><strong>一句话抓重点：</strong>${step.takeaway}</div>
    `;

    stepsEl.querySelectorAll(".il-stepper-btn").forEach((btn, btnIndex) => {
      btn.classList.toggle("active", btnIndex === index);
    });
  }

  steps.forEach((step, index) => {
    const btn = document.createElement("button");
    btn.className = "il-stepper-btn";
    btn.textContent = step.label;
    btn.addEventListener("click", () => renderStep(index));
    stepsEl.appendChild(btn);
  });

  renderStep(0);
})();
```
