# Interactive Learning Skill

一个为 **Codex CLI** 开发的 Skill，实现在 Codex CLI 内使用 Skill 功能，生成和 Claude Chat 页面中实时交互式知识块相同的学习图表与流程。

## 功能特性

### 核心能力
- **框架图（Framework Diagrams）** - 可视化概念体系、架构设计、知识框架
- **流程图（Flowcharts）** - 展示步骤流程、决策树、工作流程
- **时序图（Sequence Diagrams）** - 演示交互序列、事件时间线、系统通信过程
- **概念关系图（Concept Maps）** - 表示概念间的联系与依赖关系
- **对比图（Comparison Diagrams）** - 展示不同方案、特性对比

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
```

## 工作流程

1. **智能识别** - 自动识别学习任务，提取概念、关系、步骤和常见误区
2. **格式选择** - 根据主题特性自动选择最合适的输出格式
3. **内容生成** - 生成高质量的可视化或交互式学习组件
4. **Obsidian 优化** - 输出内容完全兼容 Obsidian，可直接插入笔记

格式路由规则：

- 如果目标明确是 Obsidian note 或 vault，交互内容默认输出 `DataviewJS`
- 如果目标不是 Obsidian 且需要可直接运行的交互成品，输出单文件 `HTML`
- 流程图在普通场景下默认优先 `SVG`
- 练习题 / 自测 / quiz 在 Obsidian 中默认优先交互式 `DataviewJS` 题卡

## 设计原则

- **聚焦学习** - 每个输出都是为了更好地理解和掌握知识
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
│   └── knowledge-point-dataviewjs-templates.md  # 交互式知识点 DataviewJS 模板集
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
