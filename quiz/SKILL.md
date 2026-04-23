---
name: quiz
description: Generate an interactive HTML quiz on a topic the user wants to practice, review, or be tested on. Use this skill whenever the user asks to make practice questions, test their knowledge, quiz themselves, or review material — phrases like "出一份...的练习题", "考考我...", "帮我复习...", "给我做个...的quiz/小测", "测测我对...的掌握", "practice", "quiz me on", "test my knowledge". Produces a self-contained HTML file with single choice, multiple choice, true/false, and fill-in-blank questions, featuring immediate feedback, scoring, timing, and shuffling. Defaults to sourcing content from the web; only uses local markdown notes when the user explicitly asks to. Saves to the Windows Desktop and opens in the default browser.
---

# Codex Quiz — 互动测验生成器

把一个主题转成一份自包含的 HTML 互动测验，保存到 Windows 桌面并用默认浏览器打开。

核心行为：**默认从网络出题**，只有用户明确要求时才从当前目录的本地笔记找内容。

---

## 何时触发

用户说出下列任一类句子（不限于字面，理解意图即可）：

- "我想练习 X"、"我想练习一下 X"
- "出一份关于 X 的练习题 / 测验 / quiz / 小测"
- "考考我 X"、"测测我 X"
- "帮我复习 X"
- "给我做个 X 的 quiz"
- "practice X", "quiz me on X", "test my knowledge of X"

**关键信号**：主题词 + 练习 / 测验 / quiz / 考 / 测 / 复习 的组合。

**不要触发**：用户只是想聊 X、想学 X、想整理 X 的笔记、问 X 是什么 —— 都不是出题。

---

## 参数解析

从用户的话里提取：

| 参数 | 默认值 | 例 |
|---|---|---|
| **主题**（必需） | — | "Redis 基础数据结构"、"React Hooks"、"光合作用" |
| **题目数量** | 10 | "5 道"、"20 题" |
| **题型偏好** | 4 种混合 | "只要选择题"、"多点填空" |
| **难度** | 中等 | "简单点"、"难一点"、"我是新手" |
| **内容来源** | **网络** | 见下节「来源判定」 |
| **输出目录** | 桌面 | "保存到 D:\\quizzes"、"放当前目录" |

主题太模糊时（只说"考考我"没带主题），反问："要考什么主题呢？"

---

## 来源判定（关键规则）

**默认：从网络获取内容出题**。不要擅自扫描本地笔记。

只有当用户**显式**说出以下意图时，才改变来源：

| 用户说法 | 来源策略 |
|---|---|
| （未指定，默认情况） | **仅网络** |
| "基于我的笔记"、"用当前目录的笔记出题"、"根据笔记内容考我" | **仅本地笔记** |
| "结合笔记和网络"、"笔记加网络资料"、"综合笔记和互联网" | **混合：笔记 + 网络** |

**关于搜索范围（本地笔记）**：

- 搜索范围 = **当前工作目录（`cwd`）及其子目录**。就这么多。
- **绝对不要**向上遍历到父目录去找 vault 根或 sibling 目录。用户在 `obsidian/redis/` 下启动 Codex，就意味着他只想让你看 redis 这个目录 —— 这是明确的上下文边界，不是让你去"帮忙"找更多内容。
- 搜本地时排除：`.` 开头的目录（`.obsidian`、`.git`、`.trash`）、`node_modules/`、符号链接

**关于"仅笔记"模式的降级处理**：

如果用户要求"仅笔记"，但笔记累计有效字数 < 200 字，不够出一份有质量的测验。此时：

1. 老实告诉用户："当前目录的相关笔记只有 X 字，不太够出 {N} 道高质量的题。"
2. 给两个选项：
   - **A**. 改为"笔记 + 网络"混合，网络内容作为补充
   - **B**. 继续只用笔记，题目数量减到 {实际能出的数量}

等用户回复再继续。

**关于网络搜索来源的优先级**：

从高到低：

1. 官方文档（redis.io、react.dev、developer.mozilla.org 等）
2. 维基百科
3. 权威教程站（MDN、W3Schools、FreeCodeCamp 等）
4. 避免：个人博客（除非特别权威）、内容农场、AI 生成文章

---

## 工作流程

### Step 1 — 解析用户请求

提取主题、题目数量、题型偏好、来源模式、输出目录。来源默认是「仅网络」。

### Step 2 — 获取内容

根据 Step 1 判定的来源模式：

**仅网络**：
- 搜索 2-3 个高质量来源覆盖主题
- 提取足够出题的事实性内容（概念、参数、关键差异、常见陷阱）
- 记录每段事实来自哪个 URL，为后面的 `source` 字段做准备

**仅笔记**：
- 在 `cwd` 及子目录里递归扫描 `.md` 文件
- 相关性排序：文件名命中 > Obsidian 标签命中 > 标题命中 > 正文命中
- 读前 N 个最相关文件的正文（跳过 front matter、图片行、纯链接）
- 累计有效字数 < 200 → 触发降级（见上一节）

**笔记 + 网络**：
- 先跑笔记扫描，理解本地有什么
- 再上网补充笔记没覆盖到的点
- 出题时两种来源按比例混搭（如笔记内容丰富就多一些笔记题，反之多一些网络题）

### Step 3 — 生成题目

**默认题型分布（10 题）**：

- 单选题 × 5
- 多选题 × 2
- 判断题 × 2
- 填空题 × 1

题目数量变化时按比例调整（如 5 题：3 单选 + 1 多选 + 1 判断）。用户有偏好就按用户的来。

**每道题必须满足**：

1. **有明确依据** —— 来自已获取的笔记或网络资料。**绝对不要凭空编造**。
2. **带解析（`explanation`）** —— 解释为什么这个答案对、错的选项错在哪。
3. **带来源（`source`）** —— 标注这道题基于哪份资料。格式：
   - 笔记：`"笔记: redis/数据结构.md"`
   - 网络：`"官方文档: redis.io/docs/data-types"`、`"MDN: Array.prototype.map"`、`"维基百科: Redis"`
4. **多选题** —— 正确答案 **≥ 2 个**。否则退化成单选。
5. **填空题** —— 答案简短（1-3 个词）。在 `acceptableAnswers` 里列出合理变体（大小写、引号、同义词）。
6. **判断题** —— 选项固定 `["对", "错"]`，answer 是 `0`（对）或 `1`（错）。

**避免的出题模式**：

- 拗口的双重否定（"下列哪个**不是**……除了……以外"）
- 脱离资料、只能靠常识猜的题
- 选项之间差别微小到只能靠运气的题
- 所有选项都对或都错的陷阱题

### Step 4 — 读取 HTML 模板

从本 skill 目录下读取模板文件：

```
assets/quiz_template.html
```

**完整路径**是本 SKILL.md 所在目录下的 `assets/quiz_template.html`。模板里有 5 个占位符需要替换（见 Step 5）。

### Step 5 — 填充模板

把模板里的 5 个占位符替换成实际内容：

| 占位符 | 替换成 |
|---|---|
| `__QUIZ_TITLE__` | 测验标题，格式 `"{主题} 练习"`（例：`"Redis 基础练习"`） |
| `__QUIZ_SUBTITLE__` | 一句话副标题（例：`"十道题，覆盖 String、Hash、List、Set、Sorted Set 的核心用法。"`） |
| `__GENERATED_AT__` | 今天的日期，格式 `"YYYY-MM-DD"` |
| `__SOURCE_LABEL__` | 来源标签：<br>• 仅网络：`"Web"` 或 `"官方文档"` 或 `"MDN + 维基百科"`<br>• 仅笔记：`"Local Notes"` 或 `"Vault: redis/"`<br>• 混合：`"Notes + Web"` |
| `__QUIZ_DATA__` | 题目数组的 JSON 字面量（**不要**加引号包裹，直接写成 `[{...}, {...}]`） |

**JSON 转义注意**：

- 题目、解析文本里如果有双引号，要转义成 `\"`
- 反斜杠要转义成 `\\`
- 换行要转义成 `\n`
- 如果题目含代码块（``` ```），保留反引号原样（字符串内合法）

### Step 6 — 保存到桌面（Windows）

**文件名**：`{主题}_quiz_{YYYYMMDD_HHMM}.html`

- 主题里的空格和 `/\:*?"<>|` 替换为下划线
- 中文保留（Windows 10/11 支持）
- 带时间戳 → 多次生成不覆盖
- 示例：`Redis_基础数据结构_quiz_20260417_1430.html`

**桌面路径（兼容 OneDrive 重定向）** —— 用 PowerShell：

```powershell
$desktop = [Environment]::GetFolderPath('Desktop')
```

或在 Git Bash / WSL 里：

```bash
DESKTOP="$USERPROFILE/Desktop"
[ -d "$USERPROFILE/OneDrive/Desktop" ] && DESKTOP="$USERPROFILE/OneDrive/Desktop"
```

写入时**一律 UTF-8 编码**（无 BOM）。

**用户指定了其他目录**时（例："保存到 D:\\quizzes" 或 "放当前目录"），用用户指定的。目录不存在就先创建。

### Step 7 — 用默认浏览器打开

**PowerShell（推荐）**：

```powershell
Start-Process $path
```

**cmd**：

```cmd
start "" "path\to\file.html"
```

### Step 8 — 回复用户（保持简短）

给用户确认信息：

- 测验标题
- 题目数量
- 保存路径
- 用了什么来源（如果来源是混合或仅笔记，列出关键笔记文件）

示例：

> 已生成《Redis 基础练习》10 道题，保存在 `C:\Users\你\Desktop\Redis_基础数据结构_quiz_20260417_1430.html`，浏览器已打开。
>
> 内容来源：Redis 官方文档（redis.io/docs/data-types）+ 维基百科。

---

## 题目 JSON Schema

```json
[
  {
    "type": "single",
    "question": "题干文字（支持 `行内代码` 和 ```代码块```）",
    "options": ["选项A", "选项B", "选项C", "选项D"],
    "answer": 1,
    "explanation": "为什么选 B，其他为什么不对。",
    "source": "笔记: path/to/file.md"
  },
  {
    "type": "multiple",
    "question": "下列哪些说法正确？",
    "options": ["A", "B", "C", "D"],
    "answer": [0, 2],
    "explanation": "A 和 C 对的理由…… B 错在…… D 错在……",
    "source": "官方文档: example.com/docs/page"
  },
  {
    "type": "truefalse",
    "question": "某某说法正确吗？",
    "options": ["对", "错"],
    "answer": 1,
    "explanation": "错。实际情况是……"
  },
  {
    "type": "blank",
    "question": "Redis 的字符串类型最大存储容量是 ______ 。",
    "answer": "512MB",
    "acceptableAnswers": ["512 MB", "512mb", "512M"],
    "explanation": "Redis 字符串最大 512MB。"
  }
]
```

字段说明：

- `type`：`"single"` | `"multiple"` | `"truefalse"` | `"blank"`
- `question`：题干，支持 `` `inline code` `` 和 ```` ``` block code ``` ```` 基本渲染
- `options`：string 数组。判断题固定 `["对", "错"]`。填空题**不写** options。
- `answer`：
  - `single` / `truefalse`：正确选项的索引（数字，从 0 开始）
  - `multiple`：正确选项索引的数组 `[0, 2]`
  - `blank`：正确答案字符串
- `acceptableAnswers`（可选，仅 `blank`）：其他合理的答案变体
- `explanation`：解析文本（必填）
- `source`：来源标注（必填）

---

## 边界情况

| 情况 | 处理 |
|---|---|
| 用户要"仅笔记"但笔记 < 200 字 | 降级处理（见上方"来源判定"） |
| 用户要"仅笔记"但 `cwd` 没有 `.md` 文件 | 告诉用户"当前目录没找到 `.md` 笔记"，问要不要改为网络 |
| 网络搜索失败（无网 / 搜不到） | 老实告诉用户"这个主题的网络资料不足，无法出题"。如果当前目录有相关笔记可以作为 fallback 提议 |
| 用户连续出同一主题 | 文件名带时间戳，不会覆盖 |
| 笔记/资料里有代码块 | 保留进题干或解析，HTML 会用 `<pre><code>` 渲染 |
| 笔记/资料里有图片 | 本版本不嵌入图片题；可以用文字描述图片内容 |
| 用户指定的目录不存在 | 先创建（PowerShell: `New-Item -ItemType Directory -Force -Path $dir`） |
| 主题与笔记关联度低（仅笔记模式） | 明确告诉用户"找到的笔记主要讲 X，不完全匹配你要的 Y，要继续吗？" |

---

## 质量自检（提交前过一遍）

- [ ] 每道题都能从笔记/网络资料里找到依据
- [ ] 多选题的正确答案 ≥ 2 个
- [ ] 填空题答案简短（1-3 个词）
- [ ] 解析解释了"为什么"，不是复读题目
- [ ] `source` 字段准确，每题都有
- [ ] 题目难度梯度合理（不全是送分或全是陷阱）
- [ ] 没有凭空编造的内容
- [ ] JSON 里的引号、反斜杠、换行都正确转义

---

## 说点题外话

这个 skill 是为 Obsidian / 笔记类 vault 设计的。用户最典型的使用场景是：打开一个特定主题的子目录（比如 `obsidian/redis/`、`obsidian/react/`），针对该主题做一轮自测。**不要越界到用户没指定的地方去找内容** —— 这个边界感对用户体验很重要。
