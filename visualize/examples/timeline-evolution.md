---
created: 2026-04-17
tags: [学习笔记, Codex生成, JavaScript, 前端历史]
topic: JavaScript 语言演进简史
---

# JavaScript 语言演进简史

> JS 从"浏览器里写点小脚本"到"跨端全栈主流语言",用了 30 年。理解它演进的脉络,能帮你理解今天为什么 JS 里到处是历史包袱和奇怪妥协。

## 为什么要看历史

读现代 JS 代码经常会困惑:为什么有 `var`、`let` 和 `const` 三种变量声明?为什么 `this` 这么反直觉?为什么有 `==` 和 `===`?为什么 Promise、async、Generator 看起来做同一件事?

**每个"奇怪"背后都有一段故事**。看完这张时间线,这些"为什么"会变成"哦,原来如此"。

## 演进时间线

<svg viewBox="0 0 820 420" xmlns="http://www.w3.org/2000/svg"
     style="width:100%;max-width:820px;height:auto;font-family:system-ui,sans-serif">

  <!-- 主时间轴 -->
  <line x1="40" y1="210" x2="780" y2="210" stroke="#D1D5DB" stroke-width="2"/>

  <!-- 时间刻度 -->
  <g font-size="10" fill="#9CA3AF" text-anchor="middle">
    <text x="80"  y="232">1995</text>
    <text x="180" y="232">2009</text>
    <text x="310" y="232">2015</text>
    <text x="440" y="232">2017</text>
    <text x="570" y="232">2020</text>
    <text x="700" y="232">2024+</text>
  </g>

  <!-- 节点(小圆点) -->
  <g>
    <circle cx="80"  cy="210" r="6" fill="#4A7DC4"/>
    <circle cx="180" cy="210" r="6" fill="#5B8F5B"/>
    <circle cx="310" cy="210" r="6" fill="#7A6BA8"/>
    <circle cx="440" cy="210" r="6" fill="#D89547"/>
    <circle cx="570" cy="210" r="6" fill="#4AA688"/>
    <circle cx="700" cy="210" r="6" fill="#C86B6B"/>
  </g>

  <!-- 事件卡片 - 上下交替,避免重叠 -->

  <!-- 1995: 诞生 (上方) -->
  <g transform="translate(20, 60)">
    <rect width="160" height="110" rx="10" fill="#E8F0FE" stroke="#4A7DC4"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">诞生</text>
    <text x="10" y="48" font-size="11" fill="#374151">Brendan Eich 花 10 天</text>
    <text x="10" y="64" font-size="11" fill="#374151">写出 Mocha → LiveScript</text>
    <text x="10" y="80" font-size="11" fill="#374151">最终命名 JavaScript</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">(跟 Java 蹭热度)</text>
  </g>
  <line x1="80" y1="170" x2="80" y2="204" stroke="#4A7DC4" stroke-width="1.5" stroke-dasharray="3,3"/>

  <!-- 2009: Node.js + ES5 (下方) -->
  <g transform="translate(120, 250)">
    <rect width="160" height="110" rx="10" fill="#E6F4E6" stroke="#5B8F5B"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">离开浏览器</text>
    <text x="10" y="48" font-size="11" fill="#374151">Node.js 发布</text>
    <text x="10" y="64" font-size="11" fill="#374151">ES5:strict mode、JSON</text>
    <text x="10" y="80" font-size="11" fill="#374151">npm 生态起飞</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">JS 成了后端语言</text>
  </g>
  <line x1="180" y1="216" x2="180" y2="250" stroke="#5B8F5B" stroke-width="1.5" stroke-dasharray="3,3"/>

  <!-- 2015: ES6 革命 (上方) -->
  <g transform="translate(240, 60)">
    <rect width="160" height="110" rx="10" fill="#EEE8F5" stroke="#7A6BA8"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">ES6 革命</text>
    <text x="10" y="48" font-size="11" fill="#374151">let / const</text>
    <text x="10" y="64" font-size="11" fill="#374151">箭头函数、class</text>
    <text x="10" y="80" font-size="11" fill="#374151">Promise、module</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">现代 JS 的起点</text>
  </g>
  <line x1="310" y1="170" x2="310" y2="204" stroke="#7A6BA8" stroke-width="1.5" stroke-dasharray="3,3"/>

  <!-- 2017: async/await (下方) -->
  <g transform="translate(380, 250)">
    <rect width="160" height="110" rx="10" fill="#FCE8CC" stroke="#D89547"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">异步成人礼</text>
    <text x="10" y="48" font-size="11" fill="#374151">async / await</text>
    <text x="10" y="64" font-size="11" fill="#374151">终结 callback hell</text>
    <text x="10" y="80" font-size="11" fill="#374151">Object rest/spread</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">异步写得像同步</text>
  </g>
  <line x1="440" y1="216" x2="440" y2="250" stroke="#D89547" stroke-width="1.5" stroke-dasharray="3,3"/>

  <!-- 2020: ES2020+ (上方) -->
  <g transform="translate(500, 60)">
    <rect width="160" height="110" rx="10" fill="#D8F0E6" stroke="#4AA688"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">ES2020+</text>
    <text x="10" y="48" font-size="11" fill="#374151">Optional chaining ?.</text>
    <text x="10" y="64" font-size="11" fill="#374151">Nullish ??</text>
    <text x="10" y="80" font-size="11" fill="#374151">BigInt、动态 import</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">语法越来越甜</text>
  </g>
  <line x1="570" y1="170" x2="570" y2="204" stroke="#4AA688" stroke-width="1.5" stroke-dasharray="3,3"/>

  <!-- 2024+: 新时代 (下方) -->
  <g transform="translate(640, 250)">
    <rect width="160" height="110" rx="10" fill="#FDE2E2" stroke="#C86B6B"/>
    <text x="80" y="24" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">平台化</text>
    <text x="10" y="48" font-size="11" fill="#374151">Bun / Deno 崛起</text>
    <text x="10" y="64" font-size="11" fill="#374151">TypeScript 事实标准</text>
    <text x="10" y="80" font-size="11" fill="#374151">WebAssembly 互操作</text>
    <text x="10" y="96" font-size="10" fill="#9CA3AF" font-style="italic">JS 不再只是语言</text>
  </g>
  <line x1="700" y1="216" x2="700" y2="250" stroke="#C86B6B" stroke-width="1.5" stroke-dasharray="3,3"/>

</svg>

## 每个阶段留下的"今天还在影响你的"

### 1995 的 10 天:为什么类型系统怪怪的

当时的目标是"让非程序员也能在网页里加点交互",所以设计得**特别宽容**:
- 类型自动转换(`"5" + 3 === "53"` 但 `"5" - 3 === 2`)
- `==` 做宽松比较(后来被 `===` 补救)
- `undefined` 和 `null` 两个都有

这些不是 bug,是当年的设计取舍,只是取的这一向到了大型项目时代变成了债。

### 2009 的 Node:为什么 JS 能在服务器跑

Chrome 的 V8 引擎被抽出来变成独立运行时,JS 突然能在没有浏览器的地方运行。从此 JS 成了全栈语言,npm 成了世界上最大的包仓库(也是最大的玩具车)。

### 2015 的 ES6:为什么有三种变量声明

`var` 是 1995 年的产物——函数作用域、可重复声明、可以在声明前使用。痛点累积了 20 年,ES6 加入 `let`(块作用域)和 `const`(不可变绑定)。**但旧代码还在用 `var`,所以三个都在**。

箭头函数也是 ES6 加的,解决了 `this` 在回调里的绑定问题。如果你经常写 `const self = this` 或 `.bind(this)`,那是因为你在看 ES5 代码。

### 2017 的 async/await:为什么异步代码有多种写法

时间线:
1. **回调**(原始时代)→ callback hell
2. **Promise**(ES6)→ `.then().then().catch()` 好一些,但链式还是丑
3. **async/await**(ES2017)→ 异步写得像同步,赢麻了

三者底层都是事件循环,但抽象层级不同。今天写代码用 async/await,但**调试时要理解 Promise**,因为 stack trace 跟着它走。

### 2020 以后:语言在"变甜"

`?.`、`??`、`?.??`、逻辑赋值 `||=`——这些都是让代码更简洁。好处是写起来舒服,成本是新人学习曲线变陡(从入门到能读现代代码的距离在拉大)。

### 2024+:JS 运行时的竞争

Node 不再一家独大:Bun 主打速度,Deno 主打标准和安全。TypeScript 几乎成了必选项。WebAssembly 让 JS 能和 Rust/Go/C++ 共存。

## 关键要点

- JS 的每个"奇怪"几乎都有历史原因——十年前的妥协,今天的债
- 读旧代码能帮你理解语言,读新代码能帮你跟上时代
- 新语法不是"炫技",多数是实际痛点积累后的回应
- 关键分水岭:**ES6(2015)**——之前是 ES5,之后是"现代 JS"

## 延伸

- 读 TC39 提案流程,看看 JS 是怎么决定加什么新特性的(Stage 0→4)
- 对比 JS 和 Python 的演进——两者的妥协点很不同
- 看看 TypeScript 是怎么在"不改 JS 语言"的前提下补足类型系统的
