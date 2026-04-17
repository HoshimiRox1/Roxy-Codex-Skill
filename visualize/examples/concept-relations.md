---
created: 2026-04-17
tags: [学习笔记, Codex生成, 分布式, CAP]
topic: CAP 定理与分布式权衡
---

# CAP 定理:为什么你不能全都要

> CAP 讲的不是"三选二"的口号,而是**在网络必然会出问题的前提下,你只能在一致性和可用性之间选一个**。这篇笔记用关系图把三者的内在联系和常见数据库的取舍画出来,帮你跳出"口号式理解"。

## 常见误读

很多人把 CAP 理解成"三个里挑两个,任选",这是错的。真正的解读是:

> **分区必然发生**(网络会挂,这不是你能选的)。所以真正的选择只有:**分区发生时,你选一致还是选可用**。

换句话说,P 是"环境条件"不是"设计选项",真正在交易的是 C 和 A。

## 三个概念的关系

<svg viewBox="0 0 600 440" xmlns="http://www.w3.org/2000/svg"
     style="width:100%;max-width:600px;height:auto;font-family:system-ui,sans-serif">

  <!-- 三个大圆(维恩图) -->
  <circle cx="220" cy="170" r="130" fill="#E8F0FE" fill-opacity="0.55" stroke="#4A7DC4" stroke-width="1.5"/>
  <circle cx="380" cy="170" r="130" fill="#E6F4E6" fill-opacity="0.55" stroke="#5B8F5B" stroke-width="1.5"/>
  <circle cx="300" cy="290" r="130" fill="#FCE8CC" fill-opacity="0.55" stroke="#D89547" stroke-width="1.5"/>

  <!-- 三字母标签 -->
  <text x="140" y="110" font-size="32" font-weight="700" fill="#4A7DC4" text-anchor="middle">C</text>
  <text x="460" y="110" font-size="32" font-weight="700" fill="#5B8F5B" text-anchor="middle">A</text>
  <text x="300" y="400" font-size="32" font-weight="700" fill="#D89547" text-anchor="middle">P</text>

  <text x="140" y="128" font-size="11" fill="#6B7280" text-anchor="middle">Consistency</text>
  <text x="460" y="128" font-size="11" fill="#6B7280" text-anchor="middle">Availability</text>
  <text x="300" y="418" font-size="11" fill="#6B7280" text-anchor="middle">Partition Tolerance</text>

  <!-- 三个重叠区域的数据库示例 -->

  <!-- C ∩ A(上方中间,没有 P 覆盖)-->
  <g>
    <rect x="258" y="150" width="84" height="24" rx="6" fill="#FFFFFF" stroke="#E5E7EB"/>
    <text x="300" y="166" font-size="11" font-weight="600" fill="#1F2937" text-anchor="middle">CA 类</text>
    <text x="300" y="196" font-size="10" fill="#6B7280" text-anchor="middle">传统 RDBMS</text>
    <text x="300" y="210" font-size="10" fill="#6B7280" text-anchor="middle">(单机 MySQL)</text>
  </g>

  <!-- C ∩ P(左下)-->
  <g>
    <rect x="183" y="255" width="84" height="24" rx="6" fill="#FFFFFF" stroke="#E5E7EB"/>
    <text x="225" y="271" font-size="11" font-weight="600" fill="#1F2937" text-anchor="middle">CP 类</text>
    <text x="225" y="298" font-size="10" fill="#6B7280" text-anchor="middle">HBase</text>
    <text x="225" y="312" font-size="10" fill="#6B7280" text-anchor="middle">MongoDB(默认)</text>
  </g>

  <!-- A ∩ P(右下)-->
  <g>
    <rect x="333" y="255" width="84" height="24" rx="6" fill="#FFFFFF" stroke="#E5E7EB"/>
    <text x="375" y="271" font-size="11" font-weight="600" fill="#1F2937" text-anchor="middle">AP 类</text>
    <text x="375" y="298" font-size="10" fill="#6B7280" text-anchor="middle">Cassandra</text>
    <text x="375" y="312" font-size="10" fill="#6B7280" text-anchor="middle">DynamoDB</text>
  </g>

  <!-- 中心不可能三角标识 -->
  <g>
    <text x="300" y="235" font-size="11" font-style="italic" fill="#9CA3AF" text-anchor="middle">三者同时不可得</text>
  </g>

</svg>

**三角的关键**:中心那块"三个全占"的区域在理论上不存在——任何分布式系统只能落在某一个两两交集里。

## 真实场景下的选择

<svg viewBox="0 0 760 280" xmlns="http://www.w3.org/2000/svg"
     style="width:100%;max-width:760px;height:auto;font-family:system-ui,sans-serif">

  <defs>
    <marker id="flow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto-start-reverse">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#9CA3AF"/>
    </marker>
  </defs>

  <!-- 起点:网络分区发生 -->
  <g transform="translate(20, 110)">
    <rect width="140" height="56" rx="10" fill="#FEF3C7" stroke="#F59E0B"/>
    <text x="70" y="26" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">网络分区</text>
    <text x="70" y="44" text-anchor="middle" font-size="11" fill="#6B7280">(必然发生)</text>
  </g>

  <!-- 分岔判断 -->
  <line x1="160" y1="138" x2="220" y2="138" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#flow)"/>
  <g transform="translate(220, 100)">
    <polygon points="60,0 120,38 60,76 0,38" fill="#EEE8F5" stroke="#7A6BA8"/>
    <text x="60" y="34" text-anchor="middle" font-size="11" fill="#1F2937">业务能接受</text>
    <text x="60" y="48" text-anchor="middle" font-size="11" fill="#1F2937">不一致吗?</text>
  </g>

  <!-- 分支:可以不一致 → AP -->
  <line x1="340" y1="120" x2="430" y2="70" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#flow)"/>
  <text x="370" y="88" font-size="10" fill="#10B981">能</text>
  <g transform="translate(440, 30)">
    <rect width="300" height="80" rx="10" fill="#E6F4E6" stroke="#5B8F5B"/>
    <text x="150" y="26" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">选 AP — 牺牲一致性</text>
    <text x="16" y="50" font-size="11" fill="#374151">• 两边继续服务,数据可能不一致</text>
    <text x="16" y="68" font-size="11" fill="#374151">• 事后通过冲突解决补齐(CRDT、向量时钟)</text>
  </g>

  <!-- 分支:不能不一致 → CP -->
  <line x1="340" y1="158" x2="430" y2="210" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#flow)"/>
  <text x="370" y="200" font-size="10" fill="#EF4444">不能</text>
  <g transform="translate(440, 170)">
    <rect width="300" height="80" rx="10" fill="#FEE2E2" stroke="#EF4444"/>
    <text x="150" y="26" text-anchor="middle" font-size="13" font-weight="600" fill="#1F2937">选 CP — 牺牲可用性</text>
    <text x="16" y="50" font-size="11" fill="#374151">• 少数派节点拒绝服务(或只读)</text>
    <text x="16" y="68" font-size="11" fill="#374151">• 多数派继续,保证写入一致</text>
  </g>

</svg>

## 判断业务属于哪一类

用几个例子训练直觉:

| 业务 | 倾向 | 理由 |
|------|------|------|
| 银行账户余额 | **CP** | 账户不一致比暂时不可用严重得多 |
| 电商商品评论 | **AP** | 看到稍旧的评论无所谓,看不到商品页很严重 |
| 分布式配置中心(ZooKeeper) | **CP** | 配置不一致会让上层服务乱套 |
| 购物车 | **AP** | 丢条商品不致命,结账时兜底校验 |
| 用户登录会话 | **AP**(多数) | 可以接受少量重复登录,但不能让用户登不上 |
| 库存扣减 | **CP** | 超卖的成本远高于短暂不可下单 |
| 社交 feed | **AP** | 看到朋友 5 秒前的动态 vs 3 秒前的动态,没区别 |

**规律**:涉及钱、库存、安全 → CP;涉及展示、娱乐、交互 → AP。

## 比 CAP 更精细的 PACELC

CAP 只说了"分区发生时"的权衡,但 99% 的时间分区没发生,这时候在权衡什么?

**PACELC 扩展版本**:
- **P** 发生时:**A** vs **C**(CAP 的原问题)
- **E**lse(没分区时):**L**atency vs **C**onsistency

也就是说,即使没分区,"每次都强一致"也会增加延迟(要等所有副本确认)。所以数据库的设计往往是:

| 数据库 | 分区时 | 无分区时 |
|--------|--------|----------|
| Cassandra | A | L(低延迟优先) |
| MongoDB | C(默认) | C(多数派写) |
| DynamoDB | A | L |
| Spanner | C | C(用 TrueTime 硬件同步) |

## 关键要点

- **P 是环境,不是选项**——真正在选的是 C 和 A
- 不是"数据库属于 CP/AP",是"这个数据库在这种配置下属于 CP/AP"——多数数据库可调
- 具体业务具体分析,别套口号
- PACELC 比 CAP 更贴近真实工程决策,推荐进一步了解

## 延伸

- 读 Eric Brewer 原论文(提出 CAP 那篇),原始表述比流行版本精确得多
- 了解 Raft / Paxos 如何在 CP 里实现可靠共识
- 了解 CRDT(无冲突复制数据类型)如何在 AP 系统里做到"最终一致"
