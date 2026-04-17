---
created: 2026-04-17
tags: [学习笔记, Codex生成, 算法, 时间复杂度]
topic: 大 O 复杂度直觉
---

# 大 O 复杂度:从"公式"到直觉

> 学过算法的人都知道 O(log n) 比 O(n) 快,但"快多少、在什么规模下开始有感"?这篇笔记用可交互的对比让你把复杂度曲线的形状记在肌肉里。

> ℹ️ 本笔记内嵌交互模块,需要启用 Obsidian 的 **Dataview 插件**(并打开 JavaScript Queries)才能看到效果。

## 常见复杂度类别

先把各阶梯列出来,后面交互里会用到:

| 类别 | 表达式 | 典型算法 | 感觉 |
|------|--------|----------|------|
| 常数 | O(1) | 哈希查找 | 永远很快 |
| 对数 | O(log n) | 二分查找、平衡树 | 几乎不增长 |
| 线性 | O(n) | 遍历、线性查找 | 匀速变慢 |
| 线性对数 | O(n log n) | 归并排序、快排平均 | 开始吃不消 |
| 平方 | O(n²) | 冒泡、选择排序 | 1 万开始卡 |
| 指数 | O(2ⁿ) | 暴力子集枚举 | 超过 30 就跑不动 |

## 交互式对比:调节 n 感受增长差异

拖动下面滑块,看看同一个 n 下各复杂度的"操作次数"有多大差距:

```dataviewjs
// ============== 容器 ==============
const root = dv.el("div", "", { cls: "viz-complexity" });
root.style.cssText = `
  padding: 18px 20px;
  border: 1px solid #E5E7EB;
  border-radius: 12px;
  background: transparent;
  font-family: system-ui, sans-serif;
  color: #1F2937;
`;

// ============== 状态 ==============
const state = { n: 20 };

// ============== 配置 ==============
const complexities = [
  { name: "O(1)",       fn: (n) => 1,                       color: "#4A7DC4", bg: "#E8F0FE" },
  { name: "O(log n)",   fn: (n) => Math.ceil(Math.log2(n)), color: "#5B8F5B", bg: "#E6F4E6" },
  { name: "O(n)",       fn: (n) => n,                       color: "#7A6BA8", bg: "#EEE8F5" },
  { name: "O(n log n)", fn: (n) => Math.ceil(n * Math.log2(n)), color: "#D89547", bg: "#FCE8CC" },
  { name: "O(n²)",      fn: (n) => n * n,                   color: "#C86B6B", bg: "#FDE2E2" },
  { name: "O(2ⁿ)",      fn: (n) => Math.pow(2, n),          color: "#4AA688", bg: "#D8F0E6" },
];

// ============== 滑块区 ==============
const sliderWrap = root.createEl("div");
sliderWrap.style.cssText = "margin-bottom: 16px;";

const sliderLabel = sliderWrap.createEl("div");
sliderLabel.style.cssText = "display:flex;justify-content:space-between;margin-bottom:8px;font-size:13px;";
const labelText = sliderLabel.createEl("span", { text: "输入规模 n" });
const labelValue = sliderLabel.createEl("span");
labelValue.style.cssText = "font-weight:600;color:#4A7DC4;font-variant-numeric:tabular-nums;";

const slider = sliderWrap.createEl("input");
slider.type = "range";
slider.min = 1;
slider.max = 40;
slider.value = state.n;
slider.style.cssText = "width:100%;accent-color:#4A7DC4;";

// ============== 结果条形区 ==============
const bars = root.createEl("div");
bars.style.cssText = "display:flex;flex-direction:column;gap:8px;";

function formatNum(x) {
  if (x < 1e4)  return x.toString();
  if (x < 1e6)  return (x / 1e3).toFixed(1) + " 千";
  if (x < 1e9)  return (x / 1e6).toFixed(1) + " 百万";
  if (x < 1e12) return (x / 1e9).toFixed(1) + " 十亿";
  return x.toExponential(1);
}

function render() {
  labelValue.textContent = `n = ${state.n}`;
  bars.innerHTML = "";

  // 计算所有值,找最大值用于比例
  const values = complexities.map(c => ({ ...c, value: c.fn(state.n) }));
  const max = Math.max(...values.map(v => Math.min(v.value, 1e15))); // 防止 2^n 溢出图
  const logMax = Math.log10(Math.max(max, 10));

  for (const c of values) {
    const row = bars.createEl("div");
    row.style.cssText = "display:flex;align-items:center;gap:10px;";

    const label = row.createEl("div", { text: c.name });
    label.style.cssText = "width:90px;font-size:12px;font-weight:600;color:#374151;font-family:ui-monospace,monospace;";

    const track = row.createEl("div");
    track.style.cssText = `
      flex:1;height:24px;background:${c.bg};border-radius:6px;position:relative;overflow:hidden;
    `;

    // 用对数刻度避免 O(2^n) 完全压扁其他条
    const logVal = Math.log10(Math.max(c.value, 1));
    const widthPct = Math.max(2, (logVal / logMax) * 100);

    const fill = track.createEl("div");
    fill.style.cssText = `
      height:100%;width:${widthPct}%;background:${c.color};opacity:0.85;
      transition:width 0.2s ease;
    `;

    const valLabel = row.createEl("div", { text: formatNum(c.value) });
    valLabel.style.cssText = `
      width:110px;text-align:right;font-size:12px;font-variant-numeric:tabular-nums;
      color:${c.color};font-weight:600;
    `;
  }
}

slider.addEventListener("input", (e) => {
  state.n = +e.target.value;
  render();
});

render();

// 提示
const hint = root.createEl("div");
hint.style.cssText = "margin-top:14px;font-size:11px;color:#9CA3AF;line-height:1.6;";
hint.innerHTML = "※ 条形长度为对数刻度,否则 O(2ⁿ) 会把其他条完全压扁。<br>※ n=30 时 O(2ⁿ) 已经超过 10 亿操作 — 实际算法题里这就叫"跑不出来"。";
```

## 观察到了什么?

这个交互的关键洞察,放在实际数据感受里就是:

- **n=10**:所有复杂度都能瞬间跑完,差别根本看不出来
- **n=20**:O(2ⁿ) 开始显著超出 O(n²),但仍都是百万级以内
- **n=30**:O(2ⁿ) 已经十亿级,一般 CPU 跑不完 1 秒
- **n=40**:O(2ⁿ) 到一万亿,O(n²) 只有 1600 — 差了**七个数量级**

所以"大 O 只看最高次项"这个规则,其实在 n 够大之后变得非常粗暴直接。n 小的时候常数差距还重要,n 大的时候阶的差距碾压一切。

## 怎么记住这个

我的建议是把**几个关键 n 的值**硬背下来:

- `log₂(10⁶) ≈ 20` — 二分在百万规模内只要 20 步
- `n log n` 在 n=10⁶ 大约 2×10⁷ — 这是快排/归并能处理的"舒适区"
- `n² = 10⁶` 时 n 只有 1000 — O(n²) 算法的可用规模上界约 10⁴
- `2²⁰ ≈ 10⁶`,`2³⁰ ≈ 10⁹` — 指数算法的极限在 n≈25

在刷算法题时看到 n 的规模,就能倒推允许的复杂度。

## 关键要点

- 复杂度不是"公式",是**曲线形状的直觉**
- n 小的时候阶差不出来,别被 O(n²) 跑小数据很快骗了
- 2 个数量级之间就是"不同物种",不存在"优化常数就赶上"

## 延伸

- Master Theorem — 推导分治算法的复杂度
- 空间复杂度同理,但多了"能否原地"的维度
- 实际性能 = 渐近复杂度 × 常数因子 × 缓存友好度 — 三者都要看
