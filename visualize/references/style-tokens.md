# Style Tokens

这里是 visualize skill 的全部色板与样式变量。**统一从这里挑**,不要随手造色号——一篇笔记里色系统一,视觉才稳。

## 核心色(所有笔记都可能用到)

| 名称 | 色值 | 用途 |
|------|------|------|
| `--text` | `#1F2937` | 主文字(深灰,不用纯黑) |
| `--text-muted` | `#6B7280` | 辅助文字、说明、箭头注释 |
| `--text-subtle` | `#9CA3AF` | 更弱的辅助文字 |
| `--border` | `#E5E7EB` | 卡片边框、分隔线 |
| `--border-strong` | `#D1D5DB` | 强调边框 |
| `--bg-subtle` | `#F9FAFB` | 弱背景填充(罕用,注意透明背景原则) |

> 背景默认透明。只有在需要"块状"区分时才给某个卡片加 background。

## 阶段色板(5 色,用于学习路径/多阶段流程)

对应 Redis 学习路径那张图的配色:

| 阶段 | bg | accent(边框/标签) | 语义暗示 |
|------|-----|---------------------|----------|
| 阶段 1 | `#E8F0FE` | `#4A7DC4` | 蓝 — 起点/基础 |
| 阶段 2 | `#E6F4E6` | `#5B8F5B` | 绿 — 进阶/稳固 |
| 阶段 3 | `#EEE8F5` | `#7A6BA8` | 紫 — 扩展/深入 |
| 阶段 4 | `#FCE8CC` | `#D89547` | 橙 — 挑战/规模 |
| 阶段 5 | `#D8F0E6` | `#4AA688` | 薄荷 — 收尾/运维 |

超过 5 个阶段时继续用:

| 阶段 6 | `#FDE2E2` | `#C86B6B` | 粉红 |
| 阶段 7 | `#E8E3D9` | `#8A7D5E` | 米 |

## 语义色(用于状态、警示、类型标记)

| 名称 | bg | accent | 用途 |
|------|-----|--------|------|
| `info` | `#E8F0FE` | `#3B82F6` | 信息、说明、中性提示 |
| `success` | `#D1FAE5` | `#10B981` | 完成、正确、通过 |
| `warning` | `#FEF3C7` | `#F59E0B` | 注意、待定、小心 |
| `danger` | `#FEE2E2` | `#EF4444` | 错误、阻塞、风险 |
| `neutral` | `#F3F4F6` | `#6B7280` | 未开始、无状态、默认 |

## 分类色板(用于对比/分类,需要区分但没有顺序含义)

当元素之间是"平行分类"(不是阶段),用这组:

| 色号 | 用途建议 |
|------|----------|
| `#60A5FA` 蓝 | 选项 A / 方案 1 |
| `#34D399` 绿 | 选项 B / 方案 2 |
| `#FBBF24` 黄 | 选项 C / 方案 3 |
| `#F87171` 红 | 选项 D / 方案 4 |
| `#A78BFA` 紫 | 选项 E / 方案 5 |
| `#FB923C` 橙 | 选项 F / 方案 6 |

搭配浅色背景时,把饱和度降一档:

| `#DBEAFE` | `#D1FAE5` | `#FEF3C7` | `#FEE2E2` | `#EDE9FE` | `#FFEDD5` |

## 字号与间距

| 用途 | 值 |
|------|-----|
| SVG 标题文字 | `font-size: 16px; font-weight: 600` |
| SVG 正文 | `font-size: 13px` |
| SVG 辅助/标签 | `font-size: 11px` |
| 卡片内边距 | `padding: 14px 18px` |
| 卡片圆角 | `border-radius: 12px` |
| 卡片间距 | 卡片之间垂直间距 `16-20px` |
| 描边粗细 | `stroke-width: 1.5` 常规 / `2` 强调 |

## 字体栈

```
font-family: system-ui, -apple-system, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif;
```

SVG 内用 `font-family="inherit"` 或显式写上面这串,不要指定具体字体(Obsidian 用户主题各异)。

## 阴影(几乎不用)

Obsidian 笔记里阴影显得花哨。如果真的需要"浮起"感,用最轻:
```
filter: drop-shadow(0 1px 2px rgba(0,0,0,0.06));
```

## 常用 SVG 片段

### 圆角卡片(带阶段 pill 标签)

```xml
<g transform="translate(40, 80)">
  <!-- 卡片底 -->
  <rect x="0" y="0" width="720" height="56" rx="12"
        fill="#E8F0FE" stroke="#4A7DC4" stroke-width="1"/>
  <!-- pill 标签 -->
  <rect x="16" y="14" width="64" height="28" rx="14" fill="#FFFFFF" opacity="0.7"/>
  <text x="48" y="33" text-anchor="middle" font-size="12" fill="#4A7DC4" font-weight="600">阶段 1</text>
  <!-- 标题 -->
  <text x="96" y="34" font-size="15" fill="#1F2937" font-weight="600">核心数据结构</text>
  <!-- 右侧时间 -->
  <text x="680" y="34" text-anchor="end" font-size="12" fill="#6B7280">3-5 天</text>
</g>
```

### 连接箭头(竖向,带注释)

```xml
<g transform="translate(40, 140)">
  <line x1="20" y1="0" x2="20" y2="20" stroke="#D1D5DB" stroke-width="1.5"/>
  <text x="40" y="14" font-size="11" fill="#6B7280">↓ 这里写阶段过渡的说明文字</text>
</g>
```

### 对比两列(表格式)

```xml
<g transform="translate(40, 40)">
  <rect x="0"   y="0" width="340" height="180" rx="12" fill="#E8F0FE" stroke="#4A7DC4"/>
  <rect x="360" y="0" width="340" height="180" rx="12" fill="#FEE2E2" stroke="#EF4444"/>
  <text x="170" y="32" text-anchor="middle" font-size="15" font-weight="600" fill="#1F2937">方案 A</text>
  <text x="530" y="32" text-anchor="middle" font-size="15" font-weight="600" fill="#1F2937">方案 B</text>
  <!-- 列表项 -->
  <text x="20"  y="70" font-size="13" fill="#374151">• 优点一</text>
  <text x="380" y="70" font-size="13" fill="#374151">• 优点一</text>
</g>
```

## 什么时候用哪套色?

| 场景 | 色板 |
|------|------|
| 学习路径、按时间推进的阶段 | 阶段色板(顺序:蓝→绿→紫→橙→薄荷) |
| 状态机、生命周期 | 语义色(start=info, active=success, blocked=warning, error=danger) |
| 对比方案、并列分类 | 分类色板 |
| 流程图的"节点"、架构图的"组件" | 核心色 + 单一分类色(保持低饱和度) |
| 时序图 | 核心色为主,每条参与者线用一个分类色区分 |

**一条铁律**:一张图里别超过 5 种色相。信息密度靠结构,不靠颜色堆砌。
