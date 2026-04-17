# visualize — Codex CLI 的可视化学习笔记 skill

把 claude.ai 网页里那种"该用图就自动出图、该用交互就自动出交互"的体验,搬到你的 codex cli,并且默认产出 **Obsidian 兼容**的 markdown 笔记(而不是独立的博客风网页)。

## 这个 skill 会做什么

你在 obsidian 的 vault 路径下打开 codex,问一个"学习/概念/架构/流程"类的问题,它会:

1. 判断这个问题是否能从视觉化中受益(该用文字就用文字,别硬塞图)
2. 起草一篇结构化的 markdown 笔记
3. 在合适的位置嵌入:
   - **静态图 / 时序 / 状态机** → 内联 SVG(Obsidian 阅读视图直接渲染)
   - **交互** → dataviewjs 代码块(需装 Dataview 插件)
4. 写到 `C:\Users\34434\OneDrive\Obsidian\学习笔记~Codex\` 下

只有当你明说"生成一个 html""给我一个单独的 svg 文件"时,才会切换到桌面输出独立文件。

## 风格

透明背景 + 粉彩色系 + 圆角卡片。和你给的 Redis 学习路径那张图一致。全部色值在 `references/style-tokens.md` 里,codex 会从那里挑,保证色系统一。

## 目录结构

```
visualize/
├── AGENTS.md                         # codex 启动时自动加载的常驻指令
├── SKILL.md                          # 主决策文档(skill 触发时读)
├── README.md                         # 你现在看的这个
├── references/
│   ├── design-thinking.md            # ⭐ 设计思维(最重要,读这个才能当得意门生)
│   ├── style-tokens.md               # 色板 / 字号 / 间距规范
│   ├── patterns.md                   # 7 种常见视觉模式的 SVG 模板(参考词汇)
│   └── embedding-guide.md            # Obsidian 嵌入的陷阱与最佳实践
└── examples/
    ├── learning-path.md              # 阶段卡片:Redis 学习路线
    ├── architecture-diagram.md       # 分层+连线:微服务架构
    ├── interactive-concept.md        # dataviewjs 滑块:大 O 复杂度
    ├── timeline-evolution.md         # 水平时间线:JavaScript 演进史
    └── concept-relations.md          # 维恩图+决策树:CAP 定理
```

每个示例代表一种**不同的设计语言**,目的是让 codex 看到视觉化不止"卡片堆叠"一种形式。

## 安装

### 第一步:放置 skill 文件

把 `visualize/` 整个文件夹放到 codex 能找到的 skills 目录:

- **Windows**: `%USERPROFILE%\.codex\skills\visualize\`(即 `C:\Users\34434\.codex\skills\visualize\`)
- **Linux/Mac**: `~/.codex/skills/visualize/`

如果你的 codex 版本用其他位置,查文档或直接问 codex:"你的 skills 目录在哪?"

### 第二步:设置 AGENTS.md(关键!)

把 `visualize/AGENTS.md` 复制到以下任一位置:

**全局生效(推荐)** — 所有 codex 会话都带着这份指令:
```
Windows: C:\Users\34434\.codex\AGENTS.md
Linux/Mac: ~/.codex/AGENTS.md
```

**项目级生效** — 只在特定项目内触发:
把 AGENTS.md 放到项目根目录(例如放到 `C:\Users\34434\OneDrive\Obsidian\学习笔记~Codex\AGENTS.md`,codex 在这个目录下启动时会自动读取)。

> **注意**:AGENTS.md 里提到的 skill 路径(默认 `~/.codex/skills/visualize/`)可能需要根据你实际放置的位置修改。打开 AGENTS.md 把 `<你放 skill 的位置>` 替换成真实路径。

### 第三步:验证安装

启动 codex,问一个测试问题:

> "帮我做一篇关于 HTTP 缓存机制的学习笔记"

如果 codex:
- ✅ 在 `学习笔记~Codex` 目录下创建了一个 `.md` 文件
- ✅ 文件里有 YAML frontmatter + 结构化章节
- ✅ 在合适位置嵌入了 SVG 图或 dataviewjs 交互块
- ✅ 风格符合 style-tokens.md(柔和色、透明背景、圆角)

那就装对了。

### 如果你的 codex 不支持 AGENTS.md

有些老版本 codex 或 fork 可能没实现这个机制。退而求其次:

1. 每次新会话开始时手动说:"请先读 `<path>/visualize/SKILL.md`,按规则处理我接下来的问题。"
2. 或者把 AGENTS.md 的内容作为系统提示(system prompt)注入(具体语法看你的 codex 版本)。

## 触发词

skill 的 description 里写了很多触发场景,但简单说:只要你问的是"概念梳理""学习路线""流程""架构""对比""状态""怎么运作",它都应该自动触发。

如果 codex 没触发,你可以主动提示:
- "按 visualize skill 的规则回答"
- "做一篇学习笔记"
- "给我一个可视化的学习路径"

## 个性化调整

路径、色板都在文件里明写了,你可以直接改:

- **输出路径**:改 `SKILL.md` 里的 "Step 1 — 输出到哪" 那一节
- **色板**:改 `references/style-tokens.md`(如果你 Obsidian 用的是深色主题,可能要调整 bg 透明度或换更深的色号)
- **笔记模板**:改 `SKILL.md` 里的"输出文件的结构模板"那一节
- **文件名规则**:同上

## 注意事项

1. **文件路径**:Windows 原生 shell 和 WSL 看到的路径不一样,SKILL.md 里有说明,codex 会先探测环境。
2. **Dataview 插件**:交互模块需要你的 Obsidian 装了 **Dataview 插件**并启用 **JavaScript Queries**(插件设置里开)。不然 dataviewjs 块只会显示为源代码。
3. **OneDrive 同步**:如果路径在 OneDrive 下,偶尔会遇到文件被同步锁定。写入失败时让 codex 重试或换路径。
4. **中文文件名**:一般没问题,OneDrive 和 Obsidian 都支持。但某些纯 POSIX 终端(locale 不对)可能乱码——这时用英文文件名。

## 设计理念

这个 skill 把我在 claude.ai 里做可视化决策的那套逻辑抽出来了:
- **默认不视觉化**,只有当图确实比文字更有效时才加
- **一篇笔记里色系统一**,不花哨
- **文字为主,图为辅**,不是反过来
- **绝不生成博客风网页**——你要的是笔记,不是作品集

希望这套 skill 能让 codex 在你的 obsidian 工作流里变成一个真正能打的学习助手。

---

*skill 版本:v1.0 · 2026-04-17*
