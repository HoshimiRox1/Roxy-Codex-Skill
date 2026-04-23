# Codex Quiz · 互动测验生成器

一个 [Codex CLI Skill](https://developers.openai.com/codex/skills)，把一个主题变成一份自包含的 HTML 互动测验，保存到桌面并用默认浏览器打开。

用自然语言告诉 Codex"我想练习一下 Redis 基本数据结构"，它会：

1. **默认从网络**（官方文档、维基百科等权威源）获取内容
2. 如果你明确说"基于笔记"，则只搜当前目录下的 `.md` 文件
3. 生成一份 HTML 测验（单选 + 多选 + 判断 + 填空）
4. 保存到桌面（兼容 OneDrive 重定向）并用默认浏览器打开

测验特性：即时反馈 + 解析、统一评分、计时、题目/选项随机打乱、键盘快捷键、再做一次。

---

## 文件结构

```
codex-quiz/
├── SKILL.md                    ← 核心 skill 指令（Codex 读这个）
├── README.md                   ← 你正在看的这个
├── example-quiz.html           ← 视觉预览（双击打开即可看效果）
└── assets/
    └── quiz_template.html      ← HTML 模板（Codex 生成测验时读取并填充）
```

---

## 安装（Windows）

Codex 的 skill 目录是 `%USERPROFILE%\.codex\skills\`。把整个 `codex-quiz` 文件夹扔进去就行：

```powershell
# PowerShell
# 假设你把解压后的 codex-quiz 文件夹放在了下载目录
Copy-Item -Recurse "$env:USERPROFILE\Downloads\codex-quiz" "$env:USERPROFILE\.codex\skills\codex-quiz"
```

或者用 File Explorer：把 `codex-quiz` 文件夹整个拖进 `%USERPROFILE%\.codex\skills\`（如果 `skills` 目录不存在手动建一下）。

**最终路径应该是**：

```
C:\Users\你\.codex\skills\codex-quiz\
├── SKILL.md
├── README.md
├── example-quiz.html
└── assets\
    └── quiz_template.html
```

安装后需要**重启 Codex**（或者新开一个 session），它启动时会自动扫描 `~/.codex/skills/` 加载所有 skill 的 name + description。

> 注：如果你的 Codex 版本需要手动启用 skills 支持，启动时加 `--enable skills` 参数（较新版本已经默认开启）。

---

## 使用

在任何目录打开 Codex（如果要用本地笔记，就在笔记所在目录打开），然后用自然语言触发：

```
# 默认（从网络出题）
你> 我想练习一下 Redis 基本数据结构
你> 出一份关于 React Hooks 的 10 道练习题
你> 考考我 SQL 连接查询，出 5 道

# 仅用当前目录的笔记
你> 基于当前目录的笔记，出一份 Redis 练习
你> 根据我的笔记考考我 React Hooks

# 结合笔记和网络
你> 结合笔记和网络出一份 Redis 数据结构的 15 道题

# 显式调用（即使 description 匹配失败也能触发）
你> $codex-quiz 出一份 Python 装饰器的题
```

Codex 会走一遍 workflow，最后告诉你保存路径和来源。

---

## 使用示例

```
你> 我想练习一下 Redis 基本数据结构

Codex> [搜索网络：Redis data types official docs]
       [搜索网络：Redis hash list set sorted set]
       [生成 10 道题，填充模板，保存文件，打开浏览器]

       已生成《Redis 基础数据结构练习》10 道题，保存在
       C:\Users\你\Desktop\Redis_基础数据结构_quiz_20260417_1430.html
       浏览器已打开。

       内容来源：Redis 官方文档 (redis.io/docs/data-types) + 维基百科。
```

```
你> 基于当前目录的笔记出 5 道 React Hooks 题

Codex> [扫描 ./*.md 和 ./**/*.md]
       [找到: useState.md, useEffect.md, 规则.md，共约 820 字]
       [生成 5 道题（3 单选 + 1 多选 + 1 填空），保存，打开]

       已生成《React Hooks 练习》5 道题，保存在
       C:\Users\你\Desktop\React_Hooks_quiz_20260417_1502.html
       浏览器已打开。

       内容来源：笔记 (useState.md, useEffect.md, 规则.md)
```

---

## 测验页面快捷键

- **<kbd>1</kbd>–<kbd>9</kbd>** — 选择对应选项（多选题可以多按几次切换）
- **<kbd>Enter</kbd>** — 提交当前题 / 进入下一题
- **<kbd>→</kbd>** — 查看解析后进入下一题

---

## 自定义视觉风格

想改样式？`assets/quiz_template.html` 是纯 HTML + CSS + 原生 JS，没有依赖（只有 Google Fonts，离线会 fallback 到系统字体）。改 CSS 变量就能换主题色：

```css
:root {
  --paper: #f5f1e8;      /* 背景（纸色） */
  --ink: #1a1a1a;        /* 正文 */
  --accent: #c1432c;     /* 主强调色（当前是砖红） */
  --success: #2d6a4f;    /* 答对（森林绿） */
  --error: #9d2626;      /* 答错（深红） */
  --serif: 'Fraunces', ...;      /* 衬线字体 */
  --sans: 'IBM Plex Sans', ...;  /* 无衬线字体 */
}
```

---

## 常见问题

**Q: Codex 没有自动触发这个 skill 怎么办？**
A: 可以用显式调用 `$codex-quiz 你的请求`。如果经常触发不了，检查一下 `SKILL.md` 是否在正确路径，description 的关键词是否和你的说法匹配。

**Q: 搜索范围能扩大到 vault 根目录吗？**
A: 默认不行 —— 这是有意设计的。用户在 `obsidian/redis/` 下打开 Codex，说明明确范围就是 redis。如果需要更大范围，在 vault 根目录启动 Codex 即可。

**Q: 能不能改成 macOS/Linux？**
A: 现在 `SKILL.md` 里 Step 6/7 的命令是按 Windows 写的。改 PowerShell → `open`（macOS）或 `xdg-open`（Linux），桌面路径改 `~/Desktop` 即可。

**Q: 公式（LaTeX）支持吗？**
A: 当前不支持。如果你的主题大量依赖公式（数学/物理），需要在 `quiz_template.html` 里加 KaTeX 或 MathJax。

**Q: 图片题支持吗？**
A: 当前版本不嵌入图片。笔记里的图片会被文字描述替代，或直接跳过。

---

## 已知限制

- Windows 默认路径写死，跨平台需手改
- 需要 Codex 能访问网络（默认模式）。如果你的 Codex 在 sandbox 模式且禁用了网络访问，请用「基于笔记」模式或临时启用网络。
- 超长笔记（> 10000 字）会被采样，不全量输入
