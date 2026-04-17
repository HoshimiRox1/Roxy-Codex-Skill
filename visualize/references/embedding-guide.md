# Obsidian 嵌入指南

Obsidian 里渲染 SVG / HTML / JS 有一些陷阱,先读完这个再动手写可以避免踩坑。

---

## Obsidian 的两种视图

笔记有两种显示状态,**渲染行为不一样**:

| 视图 | 快捷键 | 会渲染 |
|------|--------|--------|
| 阅读视图(Reading View) | `Ctrl+E` 切换 | 完整 markdown 渲染,包括内联 SVG、HTML `<details>` 等 |
| 编辑视图(Live Preview / Source) | 默认 | 大部分 markdown 渲染,**但内联 SVG 只在光标离开时渲染** |

**核心原则**:为阅读视图设计,编辑视图只是副产物。

---

## 内联 SVG — 最稳的方式

✅ **会工作**:
```markdown
## 学习路径

<svg viewBox="0 0 400 200" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="20" width="100" height="40" fill="#E8F0FE" stroke="#4A7DC4"/>
  <text x="70" y="45" text-anchor="middle" font-size="12">阶段 1</text>
</svg>
```

❌ **会出问题**:
- 把 `<style>` 标签放在 svg 外面或 markdown 文档顶部——Obsidian 经常剥离或忽略
- 用 `<link>` 引入外部 CSS——不执行
- 把 SVG 放到 ` ``` ` 代码块里——会显示成源代码而不是渲染

### 样式怎么写?

**全部内联到元素上**,不要用 `<style>`:

```xml
<!-- ❌ 不稳 -->
<svg>
  <style>.card { fill: #E8F0FE; }</style>
  <rect class="card"/>
</svg>

<!-- ✅ 稳 -->
<svg>
  <rect fill="#E8F0FE" stroke="#4A7DC4"/>
</svg>
```

对于重复元素,用 SVG 的 `<g>` 分组继承样式,或者直接复制粘贴(SVG 不长,重复几遍比起样式失效可接受)。

### hover 效果

可以用 CSS,但要写在 svg 根元素的 style 属性里:
```xml
<svg style="..." xmlns="http://www.w3.org/2000/svg">
  <style>
    .btn:hover { fill: #4A7DC4; cursor: pointer; }
  </style>
  <rect class="btn" fill="#E8F0FE"/>
</svg>
```
上面这种**嵌在 svg 内部**的 `<style>` Obsidian 一般会保留。但能不用就不用,静态笔记多数不需要 hover。

---

## Dataview 查询块

需要用户装 **Dataview 插件**。两种形态:

### dataview(声明式,简单)

查笔记元数据,展示表格/列表:

````markdown
```dataview
TABLE topic, created
FROM "学习笔记~Codex"
SORT created DESC
LIMIT 10
```
````

### dataviewjs(命令式,灵活)

需要在 Dataview 设置里启用 **"JavaScript Queries"**。

写 JS,生成 DOM:

````markdown
```dataviewjs
const container = dv.el("div", "", { cls: "viz-interactive" });
container.style.cssText = `
  padding:16px;
  border:1px solid #E5E7EB;
  border-radius:12px;
  background:transparent;
`;

const title = container.createEl("h4", { text: "复杂度计算器" });
title.style.margin = "0 0 12px 0";

// ... 构建 UI ...
```
````

**dataviewjs 的能力边界:**
- ✅ 可以创建 DOM 元素、绑定事件、做 DOM 操作
- ✅ 可以 `fetch()` 外部数据(慎用)
- ✅ 可以用 `app.vault` 操作 Obsidian 文件(强大但危险)
- ❌ 不能加载外部 npm 包(除非用户装了专门插件)
- ❌ `innerHTML` 赋值里不执行 `<script>`(安全限制)

**最佳实践:**
- 用 `document.createElement` 而不是 innerHTML
- 样式用 `element.style.cssText` 内联
- 事件用 `addEventListener`
- 状态用闭包变量,别用 window 全局

---

## 编写 dataviewjs 块的模板

### 通用骨架

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
  root.innerHTML = "";  // 重置
  // 重新构建所有元素...
}

// ============== 构建控件 ==============
const input = document.createElement("input");
input.type = "range";
input.min = 0;
input.max = 100;
input.value = state.value;
input.addEventListener("input", (e) => {
  state.value = +e.target.value;
  render();
});

// ============== 初次渲染 ==============
render();
````

### 小坑

1. **dataviewjs 在每次笔记打开时执行一次**。如果做动画或定时任务,要在 block 卸载前清理(一般不用担心,除非做很复杂的东西)。
2. **`root.innerHTML = ""`** 会移除子元素,但如果之前绑了 listener,引用要重新拿。所以**不要**把 input 放在 render 函数外面然后 render 里清空 root——会让引用失效。要么每次 render 里重建控件,要么用"局部 render"(只 patch 变化的部分)。
3. **dv.el 返回的是 container**,`dv.el("h3", "标题")` 生成 `<h3>标题</h3>` 附加到结果区域。

---

## 实用片段:检测 Dataview 是否启用

在 dataviewjs 块顶部不用判断,因为不装插件连块都不会执行。但可以在笔记**开头**放一行提示文字:

```markdown
> ℹ️ 本笔记内嵌交互模块,需要启用 Obsidian 的 **Dataview 插件**(并打开 JavaScript Queries)才能看到效果。
```

---

## Windows / WSL 路径处理

用户路径:
```
C:\Users\34434\OneDrive\Obsidian\学习笔记~Codex\
```

codex cli 运行环境可能是:

| 环境 | 写入路径 |
|------|---------|
| Windows 原生 PowerShell / cmd | `C:\Users\34434\OneDrive\Obsidian\学习笔记~Codex\xxx.md` |
| Git Bash / MSYS | `/c/Users/34434/OneDrive/Obsidian/学习笔记~Codex/xxx.md` |
| WSL | `/mnt/c/Users/34434/OneDrive/Obsidian/学习笔记~Codex/xxx.md` |

**写文件前先探测环境**(例如看 `uname`,或直接尝试 `ls` 其中一个路径),不要硬编码一种形式。

**文件名的中文**是安全的——OneDrive 和 Obsidian 都支持。但如果 codex 运行在纯 POSIX 终端且 locale 不对,可能乱码——这时可以用拼音或英文代替中文文件名,但**笔记内容**里的中文始终保留。

---

## 快速自检清单

写完笔记后扫一遍:

- [ ] 文件写在 `学习笔记~Codex` 下(或用户指定的桌面,如果是单文件导出)
- [ ] 笔记开头有 YAML frontmatter(至少 `created`, `tags`, `topic`)
- [ ] 有一句话总结(紧跟 H1 标题的 `>` 引用)
- [ ] 所有 SVG 都有 `viewBox` 和 `xmlns`
- [ ] SVG 背景透明(根元素不设 `background`)
- [ ] 所有颜色来自 tokens(没有随手写的色号)
- [ ] 没有外部 CDN `<script src>`
- [ ] 没有在 markdown 顶部写 `<style>` 块
- [ ] 如果用了 dataviewjs,开头有插件提示
- [ ] 文字为主,视觉块只在需要时插入
