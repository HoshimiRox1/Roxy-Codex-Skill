# Interactive Learning Skill

一个为 **Codex CLI** 开发的 Skill，目标是在学习、路线图、知识梳理、流程讲解等场景里，主动生成直观的学习图表与交互内容，而不是默认退回纯文字说明。

## 功能特性

### 核心能力
- **框架图（Framework Diagrams）** - 可视化概念体系、架构设计、知识框架
- **流程图（Flowcharts）** - 展示步骤流程、决策树、工作流程
- **时序图（Sequence Diagrams）** - 演示交互序列、事件时间线、系统通信过程
- **概念关系图（Concept Maps）** - 表示概念间的联系与依赖关系
- **对比图（Comparison Diagrams）** - 展示不同方案、特性对比
- **路线图（Roadmaps）** - 展示学习路径、阶段目标、能力成长路线

### 输出格式
- **SVG** - 静态精美图表，可直接在 Obsidian 中保存和查看
- **HTML + CSS + JS** - 非 Obsidian 场景下的交互式学习组件，包括标签切换、分步展示、卡片、测验等
- **DataviewJS** - Obsidian note 内的嵌入式交互式组件；只要目标是 Obsidian 交互内容，优先使用这一格式

## 使用方法

### 基本命令
```bash
$interactive-learning [你的学习主题]
```

### 示例
```
$interactive-learning 请用框架图解释 REST API 的设计原则
$interactive-learning 用流程图展示用户登录的完整步骤
$interactive-learning 画一个时序图说明微服务间的通信流程
$interactive-learning 如何从零基础成为一个合格的编程项目开发者
$interactive-learning 给我一个 6 个月的前端学习路线图
```

## 工作流程

1. **主动识别** - 自动识别学习任务，提取概念、关系、步骤和常见误区
2. **优先选图** - 如果主题更适合视觉化，直接选择最合适的图或交互组件，而不是先给一大段文字
3. **先拆阶段** - 如果是路线图类任务，先根据主题复杂度决定阶段数和阶段边界，而不是套固定 6 阶段模板
4. **直接产物** - 在普通目录下优先生成 `.svg` 或 `.html` 成品，在 Obsidian 内优先生成 `DataviewJS`
5. **轻量讲解** - 用少量补充说明服务主产物，而不是让正文压过图表本身
6. **Obsidian 优化** - 输出内容完全兼容 Obsidian，可直接插入笔记

格式路由规则：

- 如果目标明确是 Obsidian note 或 vault，交互内容默认输出 `DataviewJS`
- 如果目标不是 Obsidian 且需要可直接运行的交互成品，输出单文件 `HTML`
- 流程图在普通场景下默认优先 `SVG`
- 学习路线图 / 成长路径 / 分阶段学习计划，在普通场景下默认优先路线图式 `HTML` 或 `SVG`
- 练习题 / 自测 / quiz 在 Obsidian 中默认优先交互式 `DataviewJS` 题卡

路线图拆分原则：

- 阶段数由主题复杂度决定，不固定
- 简单主题通常 3-4 阶段就够
- 中等主题通常 4-6 阶段更清晰
- 宽广主题或职业成长路线通常 5-8 阶段更合适
- 不要为了版式好看硬凑阶段数

## 设计原则

- **聚焦学习** - 每个输出都是为了更好地理解和掌握知识
- **产物优先** - 能用图、卡片、交互块讲清的内容，不先退回纯文字
- **轻量化** - 避免冗长复杂，保持学习单元精简高效
- **无框架依赖** - 所有组件都是自包含的，无需外部 CDN 或库
- **中文优先** - 默认使用中文文本布局，支持清晰的中文呈现
- **主题自适应** - 颜色和样式设计与 Obsidian 浅色和深色主题都兼容

## 项目结构

```
interactive-learning/
├── SKILL.md              # Skill 定义和使用规则
├── agents/
│   └── openai.yaml       # Agent 配置
├── examples/
│   ├── quiz-board-template.html   # 练习题模式 HTML 样板
│   ├── quiz-board-template-dataviewjs.md  # 练习题模式 DataviewJS 样板
│   ├── knowledge-point-dataviewjs-templates.md  # 交互式知识点 DataviewJS 模板集
│   └── knowledge-shell-dataviewjs-template.md   # 通用交互式知识点高层外壳
├── references/           # 参考文档
│   ├── diagram-patterns.md        # 图表设计模式
│   ├── knowledge-point-patterns.md # 交互式知识点模式
│   ├── quiz-patterns.md           # 练习题 / 自测组件模式
│   ├── teaching-patterns.md       # 教学模式指南
│   └── obsidian-dataviewjs-patterns.md  # Obsidian 集成指南
└── README.md             # 本文件
```

## 快速开始

1. 在 Codex CLI 中调用此 Skill
2. 描述你想要学习或可视化的主题
3. Skill 会自动选择最适合的表现形式
4. 获得高质量的学习图表或交互式组件

## 支持的学习场景

- ✅ 新概念学习与讲解
- ✅ 知识体系构建
- ✅ 流程演示与指导
- ✅ 复杂系统架构设计
- ✅ 技术对比分析
- ✅ Obsidian 笔记增强
- ✅ 概念关系梳理

---

**注意** 建议在 Claude Chat 或 Codex IDE 中使用此 Skill 获得最佳体验。
