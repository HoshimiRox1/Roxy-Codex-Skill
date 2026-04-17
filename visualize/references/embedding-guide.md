# Obsidian 嵌入指南

Obsidian 里渲染 SVG / HTML / JS 有一些陷阱,先读完这个再动手写可以避免踩坑。

---

## Obsidian 的两种视图

笔记有两种显示状态,**渲染行为不一样**:

| 视图 | 快捷键 | 会渲染 |
|------|--------|--------|
| 阅读视图(Reading View) | `Ctrl+E` 切换 | 完整 markdown 渲染,但 codex 写入的内联 `<svg>` 标签经常被剥离或不渲染 |
| 编辑视图(Live Preview / Source) | 默认 | 大部分 markdown 渲染,内联 SVG 的行为也不稳定 |

**核心原则**:为阅读视图设计。由于 codex 生成的内联 `<svg>` 在阅读视图里不可靠,**本 skill 的默认策略是把所有 SVG 包到 dataviewjs 块里**,用 JS 注入 DOM。这样任何图、任何交互都用同一种嵌入形式,渲染路径统一。

---

## 默认方式:dataviewjs 块包裹 SVG

这是本 skill 在 Obsidian 输出里**唯一推荐**的 SVG 嵌入方式。

✅ **标准写法**:
````markdown
```dataviewjs
const el = dv.el("div", "", { cls: "viz-svg" });
el.innerHTML = `<svg viewBox="0 0 780 400" xmlns="http://www.w3.org/2000/svg"
  style="width:100%;max-width:780px;height:auto;font-family:system-ui,sans-serif">
  <rect x="20" y="20" width="100" height="40" rx="8" fill="#E8F0FE" stroke="#4A7DC4"/>
  <text x="70" y="45" text-anchor="middle" font-size="12" fill="#1F2937">阶段 1</text>
</svg>`;
```
````

❌ **不要这么写**(即使看起来更简洁):
```markdown
<svg viewBox="0 0 400 200" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="20" width="100" height="40" fill="#E8F0FE"/>
</svg>
```

原因:codex 直接写到 markdown 里的内联 `<svg>`,Obsidian 阅读视图经常渲染不出来。dataviewjs 里 `innerHTML` 注入的 SVG 走的是 DOM 解析路径,稳定得多。

### 前提条件

**Dataview 插件必须启用 JavaScript Queries**。笔记开头加一行提示,避免用户懵:

```markdown
> ℹ️ 本笔记的视觉块用 dataviewjs 渲染,需启用 **Dataview 插件** 的 JavaScript Queries。
```

### 常见陷阱

1. **模板字符串里的反引号**:如果 SVG 里真的需要反引号(几乎不会),切换成字符串拼接或 `String.raw` 。正常 SVG 写法不会冲突。
2. **`${}` 插值**:模板字符串里 `${}` 会被当成 JS 表达式。SVG 里不会有这种语法,但如果以后加上数据绑定要小心。
3. **SVG 属性引号**:模板字符串内部用双引号 `"` 包属性值是最稳的,不会和外层反引号冲突。
4. **样式全内联**:SVG 内部**不要**用 `<style>` 标签,所有样式直接写到元素的属性上(`fill`, `stroke`, `font-size`)或 `style=""`。原因见下一节。

### 为什么不用 `<style>` 标签?

即使在 dataviewjs 注入的 SVG 里,嵌套 `<style>` 会让整个视觉块变脆弱——某些 Obsidian 主题会和 SVG 内部样式冲突。属性化的样式写法绝对可靠:

```xml
<!-- ❌ 不稳 -->
<svg>
  <style>.card { fill: #E8F0FE; stroke: #4A7DC4; }</style>
  <rect class="card"/>
</svg>

<!-- ✅ 稳 -->
<svg>
  <rect fill="#E8F0FE" stroke="#4A7DC4"/>
</svg>
```

### hover / 动画怎么办?

如果真的需要 hover 或动画,有两条路:

1. **SVG 原生的 `<animate>`** 或 SMIL — 支持基本动画,Obsidian 能渲染
2. **用 dataviewjs 直接绑 event listener** — 用 JS 添加事件,不依赖 CSS

多数学习笔记不需要 hover/动画,保持静态就好。

---

## dataviewjs 交互块(非 SVG 场景)

如果视觉块本身就是**交互式**的(滑块、按钮、动态计算),不需要先注入 SVG,直接写 DOM 构建逻辑即可。看 `examples/interactive-concept.md`。

### 能力边界

- ✅ 可以创建 DOM 元素、绑定事件、做 DOM 操作
- ✅ 可以 `fetch()` 外部数据(慎用)
- ✅ 可以用 `app.vault` 操作 Obsidian 文件(强大但危险)
- ❌ 不能加载外部 npm 包(除非用户装了专门插件)
- ❌ `innerHTML` 赋值里不执行 `<script>` 标签(安全限制) — 但 **SVG 是可以通过 innerHTML 注入的**,SVG 不是脚本

### 最佳实践

- 用 `document.createElement` 和 `dv.el(...)` 构建结构化 DOM
- **只在注入纯静态 SVG 时用 `innerHTML`**(模板字符串拼接 SVG 字面量)
- 样式用 `element.style.cssText` 内联
- 事件用 `addEventListener`
- 状态用闭包变量,别用 window 全局

---

## Dataview 查询块(非 JS,声明式)

需要用户装 **Dataview 插件**。用于查笔记元数据、生成索引表格:

````markdown
```dataview
TABLE topic, created
FROM "学习笔记/~Codex"
SORT created DESC
LIMIT 10
```
````

> 注意 Dataview 查询里的路径分隔符用正斜杠,和文件系统路径写法不同。

---

## 编写 dataviewjs 块的模板

### 静态 SVG 最简版

````javascript
const el = dv.el("div", "", { cls: "viz-svg" });
el.innerHTML = `<svg viewBox="0 0 780 400" xmlns="http://www.w3.org/2000/svg"
  style="width:100%;max-width:780px;height:auto;font-family:system-ui,sans-serif">
  <!-- SVG 内容 -->
</svg>`;
````

### 交互式骨架

````javascript
// ============== 容器 ==============
const root = dv.el("div", "", { cls: "viz-interactive" });
root.style.cssText = `
  padding:16px 18px;
  border:1px solid #E5E7EB;
  border-radius:12px;
  background:transparent;
  font-family:system-ui,sans-serif;
  color:#1F2937;
`;

// ============== 状态 ==============
const state = { value: 0 };

// ============== 渲染函数 ==============
function render() {
  // 重新构建变化的部分...
}

// ============== 构建控件 ==============
const input = document.createElement("input");
input.type = "range";
input.addEventListener("input", (e) => {
  state.value = +e.target.value;
  render();
});

// ============== 初次渲染 ==============
render();
````

### 小坑

1. **dataviewjs 在每次笔记打开时执行一次**。如果做动画或定时任务,要在 block 卸载前清理(一般不用担心,除非做很复杂的东西)。
2. **别把 `root.innerHTML = ""` 和外部的控件引用混用** — 清空会让之前绑的 listener 失效。要么每次 render 里重建控件,要么用"局部 render"(只 patch 变化的部分)。
3. **`dv.el` 返回的是 container**,`dv.el("h3", "标题")` 生成 `<h3>标题</h3>` 附加到结果区域。

---

## 实用片段:在笔记开头写 dataviewjs 启用提示

在笔记**开头**放一行提示文字(紧跟标题的总结性 `>` 引用之后):

```markdown
> ℹ️ 本笔记的视觉块用 dataviewjs 渲染,需启用 **Dataview 插件** 的 JavaScript Queries。
```

这样用户如果没装插件,看到提示就知道去启用。

---

## Windows / WSL 路径处理

用户路径(**中间有反斜杠**,是子目录路径):
```
C:\Users\34434\OneDrive\Obsidian\学习笔记\~Codex\
```

codex cli 运行环境可能是:

| 环境 | 写入路径 |
|------|---------|
| Windows 原生 PowerShell / cmd | `C:\Users\34434\OneDrive\Obsidian\学习笔记\~Codex\xxx.md` |
| Git Bash / MSYS | `/c/Users/34434/OneDrive/Obsidian/学习笔记/~Codex/xxx.md` |
| WSL | `/mnt/c/Users/34434/OneDrive/Obsidian/学习笔记/~Codex/xxx.md` |

**写文件前先探测环境**(例如看 `uname`,或直接尝试 `ls` 其中一个路径),不要硬编码一种形式。

**特别注意**:`学习笔记\~Codex` 是两层目录,不是连写的名字。如果你看到 `学习笔记~Codex`(没有反斜杠)那是**错**的,必须修正。

**文件名的中文**是安全的——OneDrive 和 Obsidian 都支持。但如果 codex 运行在纯 POSIX 终端且 locale 不对,可能乱码——这时可以用拼音或英文代替中文文件名,但**笔记内容**里的中文始终保留。

---

## 快速自检清单

写完笔记后扫一遍:

- [ ] 文件写在 `学习笔记\~Codex` 下(路径中间有反斜杠)
- [ ] 笔记开头有 YAML frontmatter(至少 `created`, `tags`, `topic`)
- [ ] 有一句话总结(紧跟 H1 标题的 `>` 引用)
- [ ] 笔记开头有 dataviewjs 启用提示(除非完全没有视觉块)
- [ ] 所有 SVG 都在 dataviewjs 块里用 `innerHTML` 注入,没有裸露的 `<svg>` 标签
- [ ] 所有 SVG 都有 `viewBox` 和 `xmlns`
- [ ] SVG 背景透明(根元素不设 `background`)
- [ ] 所有颜色来自 tokens(没有随手写的色号)
- [ ] 没有外部 CDN `<script src>`
- [ ] 没有在 markdown 顶部写 `<style>` 块
- [ ] 没有在 SVG 内部写 `<style>` 标签
- [ ] 文字为主,视觉块只在需要时插入
- [ ] 相邻 text 的 y 坐标差够大,文字不会重叠(字号 13 用 ≥16,字号 11 用 ≥14)
