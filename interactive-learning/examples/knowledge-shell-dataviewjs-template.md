# 通用交互式知识点外壳 DataviewJS 模板

这是一份统一高层外壳模板。

目标不是把所有交互都塞进一个巨型组件，而是统一这三类常用模式的公共外层：

- `roadmap`
- `tabs`
- `stepper`

使用方式：

1. 改 `config.mode`
2. 改 `config.title` / `config.subtitle`
3. 只填当前模式对应的数据数组

```dataviewjs
(() => {
  const config = {
    mode: "tabs", // roadmap | tabs | stepper
    title: "交互式知识点通用外壳",
    subtitle: "统一容器、样式 token、事件绑定和渲染入口；具体内容只通过数据驱动。",

    roadmap: {
      phases: [
        {
          badge: "阶段 0",
          title: "准备阶段",
          meta: "1-2 天",
          summary: "先建立最基础的环境和概念。",
          items: [
            { text: "完成安装与最小可运行环境", tag: "运维" },
            { text: "跑通最小命令或 API", tag: "命令" },
            { text: "建立基础心智模型", tag: "概念" }
          ],
          open: true
        },
        {
          badge: "阶段 1",
          title: "核心知识点",
          meta: "3-5 天",
          summary: "集中掌握最常用的能力。",
          items: [
            { text: "识别核心对象与结构", tag: "概念" },
            { text: "掌握关键接口与命令", tag: "命令" },
            { text: "结合代码或业务场景使用", tag: "代码" }
          ]
        }
      ]
    },

    tabs: {
      items: [
        {
          label: "概念 A",
          color: "#2E5B9A",
          bg: "#EAF1FB",
          visual: `
            <div class="ik-mini-label">结构示意</div>
            <div class="ik-mini-box">对象 A 的简化结构或概念卡</div>
          `,
          usecase: "概念 A 适合解决的典型问题，用一句短话说明。",
          examples: [
            ["接口 A1", "说明它做什么", "CLI / shell 示例", "代码示例"],
            ["接口 A2", "说明它做什么", "CLI / shell 示例", "代码示例"]
          ],
          note: "只记一个重点时，记住这里。"
        },
        {
          label: "概念 B",
          color: "#6B4BA3",
          bg: "#F1ECFA",
          visual: `
            <div class="ik-mini-label">对比视图</div>
            <div class="ik-mini-box">对象 B 的简化结构或差异点</div>
          `,
          usecase: "概念 B 与 A 的核心区别。",
          examples: [
            ["接口 B1", "说明它做什么", "CLI / shell 示例", "代码示例"],
            ["接口 B2", "说明它做什么", "CLI / shell 示例", "代码示例"]
          ],
          note: "如果容易混淆，把误区放在这里。"
        }
      ]
    },

    stepper: {
      steps: [
        {
          label: "1. 起点",
          title: "起始状态是什么",
          diagram: [
            ["输入", "accent"],
            ["处理", "main"],
            ["结果", "accent"]
          ],
          notes: [
            "说明这个过程从哪里开始。",
            "强调当前阶段最关键的动作。"
          ],
          takeaway: "一句话抓重点：先有输入，再进入主处理。"
        },
        {
          label: "2. 中间过程",
          title: "中间如何推进",
          diagram: [
            ["状态 A", "main"],
            ["转换", "accent"],
            ["状态 B", "main"]
          ],
          notes: [
            "解释状态变化如何发生。",
            "补上容易误解的地方。"
          ],
          takeaway: "一句话抓重点：中间过程的关键是状态转换。"
        },
        {
          label: "3. 收尾",
          title: "最后如何结束",
          diagram: [
            ["当前状态", "main"],
            ["收尾动作", "accent"],
            ["最终状态", "main"]
          ],
          notes: [
            "解释最终状态为什么重要。",
            "说明这一段对整体理解的价值。"
          ],
          takeaway: "一句话抓重点：最终状态决定整个过程如何被理解。"
        }
      ]
    }
  };

  const root = dv.el("div", "");
  root.className = "ik-shell";

  root.innerHTML = `
    <style>
      .ik-shell {
        --ik-border: var(--background-modifier-border);
        --ik-text: var(--text-normal);
        --ik-muted: var(--text-muted);
        --ik-faint: var(--text-faint);
        --ik-accent: var(--interactive-accent);
        font-family: var(--font-sans, sans-serif);
        margin: 1rem 0;
        color: var(--ik-text);
      }
      .ik-wrap {
        border: 1px solid var(--ik-border);
        border-radius: 18px;
        padding: 16px;
        background: color-mix(in srgb, var(--background-primary) 96%, #7f8ea3 4%);
      }
      .ik-head {
        margin-bottom: 14px;
      }
      .ik-title {
        font-size: 16px;
        font-weight: 700;
        margin-bottom: 6px;
      }
      .ik-subtitle {
        font-size: 12.8px;
        line-height: 1.7;
        color: var(--ik-muted);
      }
      .ik-mini-label {
        font-size: 12px;
        color: var(--ik-faint);
        margin-bottom: 8px;
      }
      .ik-mini-box {
        padding: 10px 12px;
        border-radius: 12px;
        border: 1px solid var(--ik-border);
        background: color-mix(in srgb, var(--background-primary) 94%, #7f8ea3 6%);
      }
      .ik-roadmap-phase {
        border: 1px solid var(--ik-border);
        border-radius: 14px;
        overflow: hidden;
        margin-bottom: 12px;
        background: color-mix(in srgb, var(--background-primary) 98%, #6b7a8f 2%);
      }
      .ik-roadmap-head {
        display: flex;
        gap: 10px;
        align-items: center;
        padding: 12px 14px;
        cursor: pointer;
      }
      .ik-roadmap-badge {
        font-size: 11px;
        font-weight: 700;
        border-radius: 999px;
        padding: 4px 9px;
        color: var(--ik-accent);
        background: color-mix(in srgb, var(--ik-accent) 16%, transparent);
      }
      .ik-roadmap-title {
        flex: 1;
        font-size: 14px;
        font-weight: 700;
      }
      .ik-roadmap-meta {
        font-size: 12px;
        color: var(--ik-muted);
      }
      .ik-roadmap-chevron {
        font-size: 12px;
        color: var(--ik-faint);
        transition: transform .18s ease;
      }
      .ik-roadmap-chevron.open {
        transform: rotate(90deg);
      }
      .ik-roadmap-body {
        display: none;
        padding: 0 14px 14px;
      }
      .ik-roadmap-body.open {
        display: block;
      }
      .ik-roadmap-summary {
        margin: 4px 0 10px;
        color: var(--ik-muted);
        font-size: 13px;
        line-height: 1.7;
      }
      .ik-roadmap-list {
        list-style: none;
        margin: 0;
        padding: 0;
      }
      .ik-roadmap-item {
        display: flex;
        gap: 10px;
        align-items: flex-start;
        padding: 7px 0;
      }
      .ik-roadmap-dot {
        width: 6px;
        height: 6px;
        border-radius: 50%;
        margin-top: 7px;
        background: var(--ik-accent);
        flex-shrink: 0;
      }
      .ik-roadmap-item-text {
        flex: 1;
        font-size: 13px;
        line-height: 1.6;
      }
      .ik-roadmap-tag {
        font-size: 11px;
        color: var(--ik-muted);
        border: 1px solid var(--ik-border);
        border-radius: 999px;
        padding: 2px 7px;
        white-space: nowrap;
      }
      .ik-tabs-row,
      .ik-stepper-steps {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
        margin-bottom: 12px;
      }
      .ik-tab,
      .ik-step-btn {
        border: 1px solid var(--ik-border);
        background: transparent;
        color: var(--ik-text);
        border-radius: 999px;
        padding: 7px 12px;
        cursor: pointer;
        font-size: 12px;
        font-weight: 700;
      }
      .ik-tab-panel,
      .ik-step-panel {
        display: none;
        border: 1px solid var(--ik-border);
        border-radius: 16px;
        padding: 14px;
        background: color-mix(in srgb, var(--background-primary) 98%, #6b7a8f 2%);
      }
      .ik-tab-panel.active,
      .ik-step-panel.active {
        display: block;
      }
      .ik-tab-usecase {
        font-size: 13px;
        line-height: 1.7;
        margin: 12px 0;
      }
      .ik-example-list {
        list-style: none;
        margin: 0;
        padding: 0;
      }
      .ik-example-item {
        border-top: 1px solid var(--ik-border);
        padding: 10px 0;
      }
      .ik-example-main {
        font-size: 13px;
      }
      .ik-example-toggle {
        display: inline-flex;
        align-items: center;
        padding: 2px 8px;
        border-radius: 999px;
        font-size: 11px;
        font-weight: 700;
        cursor: pointer;
      }
      .ik-example-body {
        display: none;
        margin-top: 8px;
        font-size: 12px;
        line-height: 1.7;
        color: var(--ik-muted);
      }
      .ik-example-item.open .ik-example-body {
        display: block;
      }
      .ik-note {
        margin-top: 10px;
        padding: 8px 12px;
        border-left: 2px solid var(--ik-border);
        color: var(--ik-muted);
        font-size: 12.5px;
      }
      .ik-step-scene {
        display: grid;
        grid-template-columns: 1.1fr 1fr;
        gap: 14px;
      }
      .ik-step-diagram {
        border: 1px dashed var(--ik-border);
        border-radius: 12px;
        padding: 12px;
      }
      .ik-step-diagram-title {
        font-size: 13px;
        font-weight: 700;
        margin-bottom: 10px;
      }
      .ik-step-flow {
        display: flex;
        gap: 8px;
        flex-wrap: wrap;
        align-items: center;
      }
      .ik-step-node {
        border-radius: 10px;
        padding: 8px 12px;
        font-size: 12.5px;
        font-weight: 700;
        border: 1px solid transparent;
      }
      .ik-step-node.main {
        background: color-mix(in srgb, var(--background-primary) 84%, #7ca7d5 16%);
      }
      .ik-step-node.accent {
        background: color-mix(in srgb, var(--background-primary) 84%, #8ec8aa 16%);
      }
      .ik-step-arrow {
        color: var(--ik-muted);
      }
      .ik-step-notes h4 {
        margin: 0 0 8px;
        font-size: 13px;
      }
      .ik-step-notes ul {
        margin: 0;
        padding-left: 18px;
      }
      .ik-step-notes li {
        margin-bottom: 8px;
        font-size: 12.8px;
        line-height: 1.65;
      }
      .ik-step-takeaway {
        margin-top: 12px;
        padding: 10px 12px;
        border-left: 3px solid var(--ik-accent);
        font-size: 12.8px;
        line-height: 1.7;
        background: color-mix(in srgb, var(--background-primary) 94%, #8ec8aa 6%);
      }
      @media (max-width: 860px) {
        .ik-step-scene {
          grid-template-columns: 1fr;
        }
      }
    </style>
    <section class="ik-wrap">
      <div class="ik-head">
        <div class="ik-title">${config.title}</div>
        <div class="ik-subtitle">${config.subtitle}</div>
      </div>
      <div class="ik-body"></div>
    </section>
  `;

  const body = root.querySelector(".ik-body");

  function renderRoadmap() {
    body.innerHTML = config.roadmap.phases.map((phase) => `
      <section class="ik-roadmap-phase">
        <div class="ik-roadmap-head">
          <span class="ik-roadmap-badge">${phase.badge}</span>
          <span class="ik-roadmap-title">${phase.title}</span>
          <span class="ik-roadmap-meta">${phase.meta}</span>
          <span class="ik-roadmap-chevron ${phase.open ? "open" : ""}">▶</span>
        </div>
        <div class="ik-roadmap-body ${phase.open ? "open" : ""}">
          <div class="ik-roadmap-summary">${phase.summary}</div>
          <ul class="ik-roadmap-list">
            ${phase.items.map((item) => `
              <li class="ik-roadmap-item">
                <span class="ik-roadmap-dot"></span>
                <span class="ik-roadmap-item-text">${item.text}</span>
                <span class="ik-roadmap-tag">${item.tag}</span>
              </li>
            `).join("")}
          </ul>
        </div>
      </section>
    `).join("");
  }

  function renderTabs() {
    body.innerHTML = `
      <div class="ik-tabs-row"></div>
      <div class="ik-tab-panels"></div>
    `;
    const tabsRow = body.querySelector(".ik-tabs-row");
    const panels = body.querySelector(".ik-tab-panels");

    function activate(index) {
      tabsRow.querySelectorAll(".ik-tab").forEach((tab, i) => {
        const active = i === index;
        const topic = config.tabs.items[i];
        tab.style.background = active ? topic.color : "";
        tab.style.borderColor = active ? topic.color : "";
        tab.style.color = active ? "#fff" : "";
      });
      panels.querySelectorAll(".ik-tab-panel").forEach((panel, i) => {
        panel.classList.toggle("active", i === index);
      });
    }

    config.tabs.items.forEach((topic, index) => {
      const tab = document.createElement("button");
      tab.className = "ik-tab";
      tab.textContent = topic.label;
      tab.addEventListener("click", () => activate(index));
      tabsRow.appendChild(tab);

      const panel = document.createElement("section");
      panel.className = "ik-tab-panel";
      panel.innerHTML = `
        <div class="ik-tab-vis">${topic.visual}</div>
        <div class="ik-tab-usecase">${topic.usecase}</div>
        <ul class="ik-example-list">
          ${topic.examples.map(([name, desc, cli, code]) => `
            <li class="ik-example-item">
              <div class="ik-example-main">
                <span class="ik-example-toggle" tabindex="0" role="button" style="background:${topic.bg};color:${topic.color}">${name}</span>
                ：${desc}
              </div>
              <div class="ik-example-body">
                <div><strong>CLI</strong>：${cli}</div>
                <div><strong>代码</strong>：${code}</div>
              </div>
            </li>
          `).join("")}
        </ul>
        <div class="ik-note">${topic.note}</div>
      `;
      panels.appendChild(panel);
    });

    body.addEventListener("click", (event) => {
      const toggle = event.target.closest(".ik-example-toggle");
      if (!toggle) return;
      const item = toggle.closest(".ik-example-item");
      if (!item) return;
      item.classList.toggle("open");
    });

    body.addEventListener("keydown", (event) => {
      const toggle = event.target.closest(".ik-example-toggle");
      if (!toggle) return;
      if (event.key !== "Enter" && event.key !== " ") return;
      event.preventDefault();
      const item = toggle.closest(".ik-example-item");
      if (!item) return;
      item.classList.toggle("open");
    });

    activate(0);
  }

  function renderStepper() {
    body.innerHTML = `
      <div class="ik-stepper-steps"></div>
      <div class="ik-stepper-panels"></div>
    `;
    const stepsRow = body.querySelector(".ik-stepper-steps");
    const panels = body.querySelector(".ik-stepper-panels");

    function activate(index) {
      stepsRow.querySelectorAll(".ik-step-btn").forEach((btn, i) => {
        btn.classList.toggle("active", i === index);
      });
      panels.querySelectorAll(".ik-step-panel").forEach((panel, i) => {
        panel.classList.toggle("active", i === index);
      });
    }

    config.stepper.steps.forEach((step, index) => {
      const btn = document.createElement("button");
      btn.className = "ik-step-btn";
      btn.textContent = step.label;
      btn.addEventListener("click", () => activate(index));
      stepsRow.appendChild(btn);

      const panel = document.createElement("section");
      panel.className = "ik-step-panel";
      panel.innerHTML = `
        <div class="ik-step-scene">
          <div class="ik-step-diagram">
            <div class="ik-step-diagram-title">${step.title}</div>
            <div class="ik-step-flow">
              ${step.diagram.map(([text, type], i) => `
                <span class="ik-step-node ${type}">${text}</span>
                ${i < step.diagram.length - 1 ? '<span class="ik-step-arrow">→</span>' : ''}
              `).join("")}
            </div>
          </div>
          <div class="ik-step-notes">
            <h4>这一阶段在说明什么</h4>
            <ul>${step.notes.map((note) => `<li>${note}</li>`).join("")}</ul>
          </div>
        </div>
        <div class="ik-step-takeaway"><strong>一句话抓重点：</strong>${step.takeaway}</div>
      `;
      panels.appendChild(panel);
    });

    activate(0);
  }

  if (config.mode === "roadmap") {
    renderRoadmap();
  } else if (config.mode === "tabs") {
    renderTabs();
  } else if (config.mode === "stepper") {
    renderStepper();
  } else {
    body.innerHTML = `<div class="ik-note">未知模式：${config.mode}</div>`;
  }

  root.addEventListener("click", (event) => {
    const head = event.target.closest(".ik-roadmap-head");
    if (!head) return;
    const phase = head.closest(".ik-roadmap-phase");
    const bodyEl = phase?.querySelector(".ik-roadmap-body");
    const chevron = phase?.querySelector(".ik-roadmap-chevron");
    if (!bodyEl || !chevron) return;
    const isOpen = bodyEl.classList.toggle("open");
    chevron.classList.toggle("open", isOpen);
  });
})();
```
