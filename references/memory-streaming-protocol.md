# 记忆流协议（Memory Streaming Protocol · SSOT）

> **目标**：让 35 层记忆体系**按需流式加载**，而非每次全读。
> 三个核心机制：(a) Hot/Warm/Cold 三级分级；(b) FULL/HEAD/DELTA 三种粒度；(c) Memory Stream（每章 delta 增量流）。
> 配合既有 [memory-layer-index.md](./memory-layer-index.md) 使用 —— 该文件管"哪层做什么"，本文件管"哪层怎么读多少"。

---

## 一、问题与目标

### 现状痛点

35 层记忆体系按 A/B/C 三级加载，看似分级，但实际：
1. 每层默认整文件 FULL 读，长篇 50+ 章后单层文件可能超 5K tokens
2. 无 token 预算，A 级 7 层全读累计可达 15K-30K tokens，挤占正文生成空间
3. 无变化频率感知 —— 不变的 worldbuilding 与每章变的 chapter-summary 享受同样加载权重
4. 无 delta 概念 —— 每章续写都重新读一遍 Canon、时间线、章节摘要，浪费

### 设计目标

| # | 目标 | 量化指标 |
|---|------|---------|
| 1 | A 级 7 层加载总 token 数（Standard 模式） | ≤ 5000 tokens |
| 2 | A 级 7 层加载总 token 数（Lite 模式） | ≤ 2000 tokens |
| 3 | A 级 7 层加载总 token 数（Pro 模式） | ≤ 10000 tokens |
| 4 | 续写时 delta 加载占比 | ≥ 50%（即静态部分 ≤ 50%） |
| 5 | 长篇 ≥ 50 章项目，记忆加载不随章节数线性膨胀 | 50 章 vs 5 章加载 token 差 ≤ 30% |

---

## 二、Hot / Warm / Cold 三级分级（35 层完整映射）

### 2.1 分级原则

| 分级 | 含义 | 默认粒度 | 何时重读 |
|------|------|---------|---------|
| 🔥 **HOT** | 每章变化、必读 | HEAD（最近 N 条 / 当前章相关） | 每章 |
| 🌡️ **WARM** | 中频访问，按任务匹配 | HEAD（项目级硬约束）或 DELTA（本章涉及部分） | 任务匹配时 |
| 🧊 **COLD** | 静态参考，罕变 | FULL（一次读，缓存）或 HEAD | 仅初始化 / 重大变更后 |

### 2.2 35 层 Hot/Warm/Cold 完整表

#### A 级（7 层）

| # | 名称 | 分级 | 默认粒度 | 备注 |
|---:|------|:----:|:--------:|------|
| 10 | 记忆索引 | 🔥 HOT | FULL | 入口，必每次重读 |
| 1 | Canon 事实表 | 🌡️ WARM | HEAD（本章涉及） | 项目级硬约束部分 FULL，其余 DELTA |
| 3 | 时间线 | 🔥 HOT | HEAD（最近 5 章 + canon + future） | 长篇可只读最近窗口 |
| 2 | 人物状态表 | 🔥 HOT | DELTA（最近 3 章变化） | 静态角色基础卡走 COLD |
| 5 | 章节摘要 | 🔥 HOT | HEAD（最近 3 章） | 早章节摘要走 COLD |
| 28 | 章节对齐单 | 🔥 HOT | HEAD（仅当前 + 上一章） | |
| 29 | 十章呼应板 | 🔥 HOT | HEAD（最近 10 章） | |

#### B 级（17 层）

| # | 名称 | 分级 | 默认粒度 |
|---:|------|:----:|:--------:|
| 4 | 伏笔与线索表 | 🌡️ WARM | HEAD（活跃伏笔） |
| 6 | 势力棋盘 | 🌡️ WARM | HEAD |
| 7 | 结构网图 | 🧊 COLD | FULL（一次读） |
| 11 | 连续性台账 | 🌡️ WARM | DELTA |
| 12 | 群像总表 | 🌡️ WARM | HEAD |
| 13 | 群像弧光矩阵 | 🌡️ WARM | DELTA |
| 14 | 未知钩子矩阵 | 🌡️ WARM | HEAD（未揭示部分） |
| 15 | 主角主动性追踪 | 🌡️ WARM | DELTA |
| 21 | 多阵营动态表 | 🌡️ WARM | DELTA |
| 25 | 冲突节奏板 | 🌡️ WARM | HEAD（最近 N 章） |
| 27 | 金句与有趣角色单 | 🌡️ WARM | HEAD |
| 30 | 支线脆弱度表 | 🌡️ WARM | HEAD（即将超时项） |
| 31 | 场景骨架表 | 🔥 HOT | HEAD（仅本章场景） |
| 32 | 信息流控制单 | 🌡️ WARM | HEAD（本章 + 待释放） |
| 33 | 节奏控制单 | 🌡️ WARM | HEAD |
| 34 | 角色驱动单 | 🌡️ WARM | DELTA |
| 35 | 质量闭环单 | 🌡️ WARM | DELTA（最近 1-2 章） |

#### C 级（11 层）

| # | 名称 | 分级 | 默认粒度 |
|---:|------|:----:|:--------:|
| 8 | 宇宙设定 | 🧊 COLD | FULL（一次读，缓存） |
| 9 | 地图体系 | 🧊 COLD | FULL（一次读，缓存） |
| 16 | 投稿润色单 | 🌡️ WARM | HEAD |
| 17 | 去 AI 味诊断单 | 🌡️ WARM | DELTA |
| 18 | 章首轮换表 | 🔥 HOT | HEAD（连载项目升 HOT） |
| 19 | 章首第一句表 | 🌡️ WARM | HEAD |
| 20 | 单章字数控制单 | 🌡️ WARM | HEAD |
| 22 | 情绪爆点表 | 🌡️ WARM | HEAD |
| 23 | 对白声线矩阵 | 🧊 COLD | FULL（一次读，缓存） |
| 24 | 情绪价值板 | 🧊 COLD | HEAD |
| 26 | 魅力角色表 | 🧊 COLD | FULL（一次读，缓存） |

### 2.3 分级动态升降规则

| 触发 | 调整 |
|------|------|
| 连载项目（多章持续推进） | 层 18 章首轮换表 自动 COLD → HOT |
| 项目刚启动（≤ 3 章） | A 级所有 DELTA → HEAD（无 delta 历史） |
| 上下文 > 20 轮 | WARM 层降级为按需触发；COLD 层缓存命中即跳过 |
| rollback_pending=true | A 级所有 DELTA → FULL（必须全验） |
| 用户切换大纲 / 主线 | 涉及层 6/7/4 自动 COLD/WARM → HOT 一次 |

---

## 三、FULL / HEAD / DELTA 三种粒度

### 3.1 粒度定义

| 粒度 | 读取范围 | Token 成本 | 适用 |
|------|---------|----------:|------|
| **FULL** | 整个文件全文 | 高（1-5K tokens / 文件） | 重大重读、初始化、回滚 |
| **HEAD** | 头部 / 最近 N 条 / 项目级硬约束部分 | 中（300-1500 tokens） | 默认每次 |
| **DELTA** | 自上次读取以来的变化（从 memory-stream 读取） | 低（50-300 tokens） | 续写时 |

### 3.2 HEAD 模式细节

每种状态文件的 HEAD 范围定义：

| 状态文件 | HEAD 范围 |
|---------|---------|
| canon-facts | 项目级硬约束（前 30%）+ 本章涉及条目 |
| timeline | 最近 5 章 major + 全部 canon + 全部 future（未发生） |
| chapter-summary | 最近 3 章 |
| ten-chapter-echo | 全部（已经是 10 章窗口）|
| chapter-alignment | 当前章 + 上一章 |
| 章首轮换表 | 最近 6 章 |
| 字数控制单 | 当前章 |

### 3.3 DELTA 模式细节

DELTA 不直接从原文件读，而是从 memory-stream 读最近 N 章变更：

```
DELTA 加载流程：
1. 读 memory-stream HEAD（最近 N 章 entry）
2. 提取目标层的 delta 段（如 timeline / canon）
3. 与上次缓存的 HEAD 合并
4. 输出当前最新视图
```

DELTA 模式将"读全表"变成"读最近变化"，token 节省 60-80%。

---

## 四、Token 预算（按执行模式）

| 模式 | A 级总预算 | 单层上限 | WARM 层动态预算 | COLD 层 | 第 36 层（deep-memory）|
|------|----------:|---------:|----------------:|--------:|---------------:|
| **Lite** | 2000 | 500 | 不加载 | 不加载 | 200（仅 dedup-ledger） |
| **Standard** | 5000 | 1500 | 3000 | 仅初始化加载 1 次 | 800（8 维 HEAD） |
| **Pro** | 10000 | 3000 | 6000 | 按需加载 | 2000（8 维 FULL） |
| **Q（日更续写）** | 1500 | 400 | 不加载（Q 模式仅 A 级 + 章节卡） | 不加载 | 300（仅 dedup + promise-ledger 活跃项） |

### 4.1 超预算时的压缩协议

```
预算超额检测：
1. 测算本次加载总 token 数
2. 若 > 模式预算 → 触发压缩
3. 压缩顺序：
   ① COLD 层 FULL → HEAD（保留前 30%）
   ② WARM 层 HEAD → DELTA（仅最近 5 章）
   ③ HOT 层 HEAD 缩窗（最近 N 章 → 最近 N/2 章）
   ④ 若仍超 → 用户提示，请求人工裁剪
4. 压缩日志写入 gate-log §执行信息"裁剪记录"
```

### 4.2 缓存协议（COLD 层）

COLD 层读取后缓存于会话上下文，下次同会话内不再重读。
缓存失效条件：
- 文件被 git 修改（mtime 变更）→ 失效
- 用户显式说"重读全部记忆"
- rollback_pending=true 触发 → 失效

---

## 五、Memory Stream 文件格式（07-memory/memory-stream.md）

### 5.1 路径与生命周期

- **路径**：`{项目根}/07-memory/memory-stream.md`
- **创建时机**：项目第 1 章门 5 回写时自动建
- **更新时机**：每章门 5 回写后追加一条 entry
- **不删历史**：按章节顺序追加，永不删除已写入的 entry

### 5.2 单 entry 格式

```markdown
## CH{N} (YYYY-MM-DDTHH:MM)

canon:
- 新增：{条目描述}
- 修改：{条目} {old → new}

timeline:
- {时间点}｜{事件}｜地点 {地点}｜类型 {canon/major/subplot/...}

characters:
- {角色}：{状态变化描述}
- {角色}：{关系变化}

foreshadow:
- {伏笔 ID}：{埋点 / 偏移 / 回收}
- 新增：{伏笔描述}

worldmap:
- 新地点：{名称} ({区域 / 坐标})
- 区域调整：{区域名} {变化}

universemap:
- 跨界事件：{事件}
- 通路变化：{描述}
（单世界故事此段可省略）
```

### 5.3 无变化条目处理

各段无变化时填 `—` 占一行，不删段：

```markdown
## CH{N} (timestamp)
canon: —
timeline: [...]
characters: —
foreshadow: —
worldmap: —
universemap: —
```

确保解析器始终找到 6 段。

### 5.4 与 gate-log 的联动

`05-chapters/gate-logs/CH{N}-gate-log.md` 末尾的"记忆流 delta"段是本 entry 的草稿。门 5 回写时**复制**到 `07-memory/memory-stream.md`，并加 entry header `## CH{N} (timestamp)`。

---

## 六、记忆加载默认序列

### 6.1 标准加载（Standard 模式）

```
1. memory-stream HEAD（最近 5 章 entry）→ 200-500 tokens
2. canon HEAD（项目级硬约束 + 本章涉及） → 500 tokens
3. timeline HEAD（最近 5 章 + canon major + future） → 500 tokens
4. memory-index → 100 tokens
5. 当前章节卡 → 300 tokens
6. chapter-alignment（上一章 + 当前）→ 200 tokens
7. ten-chapter-echo HEAD → 500 tokens
   — 小计 A 级 ~2300-2600 tokens
8. 按 task 触发 WARM 层（见 §6.3） → 0-3000 tokens
9. COLD 层不读，按需触发
   — 总 ~2300-5500 tokens（在 5000 预算内）
```

### 6.1.1 第 36 层 deep-memory 分级映射（与 §二.2 A/B/C 正交）

| deep-memory 子维度 | tier | 默认粒度 | 加载触发 |
|-------------------|:----:|:--------:|---------|
| 8.1 主题母题 | 🌡️ WARM | HEAD（活跃 5 项）| 门 1 + 门 4 |
| 8.2 关系弧光 | 🔥 HOT | HEAD（当前阶段）| 门 1 |
| 8.3 知识状态图谱 | 🔥 HOT | HEAD（本章涉及） | 门 1 + 门 4 P0#9 C3 |
| 8.4 承诺账本 | 🔥 HOT | HEAD（active + 即将超期）| 门 1 + 门 4 P0#5 |
| 8.5 读者体验指纹 | 🌡️ WARM | HEAD（最近 5 章） | 门 4 |
| 8.6 风格指纹 | 🧊 COLD | FULL（每 5 章重测） | 每 5 章 1 次 |
| 8.7 隐性后果 | 🔥 HOT | HEAD（must + 范围内 high）| 门 1 |
| 8.8 重复声明记录 | 🔥 HOT | FULL（dedup-ledger 全表）| 门 3 实时检测 |

### 6.2 Lite 模式（短篇 / 试写）

```
1. memory-stream HEAD（最近 1-2 章） → 100 tokens
2. canon HEAD → 300 tokens
3. timeline HEAD → 300 tokens
4. memory-index → 100 tokens
5. 当前章节卡 → 300 tokens
   — 总 ~1100 tokens（在 2000 预算内）
WARM/COLD 不加载
```

### 6.2.1 Q 模式（日更续写）加载序列

```
1. memory-stream HEAD（最近 3 章 entry，含 deep delta） → 200 tokens
2. canon HEAD（项目级硬约束） → 300 tokens
3. timeline HEAD（最近 3 章 + future 已超期）→ 200 tokens
4. memory-index → 100 tokens
5. 最新章节卡 → 200 tokens
6. dedup-ledger HEAD（活跃简称）→ 100 tokens
7. promise-ledger（active + 即将超期）→ 200 tokens
   — 总 ~1300 tokens（在 1500 预算内）
WARM/COLD 不加载；不读 5 章 chapter-summary
触发自动判断：用户输入是 1 句话方向指令 → 自动进入 Q 模式
```

### 6.3 触发器（Standard 自动加载 WARM）

| 触发条件 | 自动加载 |
|---------|---------|
| 本章涉及新角色 | character-template + relationship-map |
| 本章涉及新地点 | world-map 区域 |
| 本章触发未知钩子 | unknown-hook-matrix |
| 群像章节（≥3 角色独立行动） | ensemble-cast + ensemble-arc-matrix |
| 多阵营冲突 | multi-faction-dynamics + faction-board |
| 高情绪段 | emotion-burst + emotional-value-board |
| 章首手法待选 | first-sentence-template + chapter-opening-rotation |

### 6.4 重大事件触发（Pro 模式自动加载 COLD）

| 触发 | 加载 |
|------|------|
| 进入新世界 / 新位面 | universe-map（FULL） |
| 主角晋升 / 体系突破 | 宇宙设定 + 力量体系（FULL） |
| 跨大区移动 | world-map FULL |

---

## 七、压缩协议（细节）

### 7.1 行级压缩

- 删冗余空行（≥ 2 连续空行 → 1 个）
- 删示例占位行（`待定` / `...` / 模板 placeholder）
- 合并相邻同类条目（如 timeline 同一时间点多事件 → 合并）

### 7.2 字段级压缩

- 隐藏 `background` 类型 timeline 事件（除非 §零 标 canon）
- 隐藏 `future` 已超时未触发的事件
- 隐藏被覆盖的 Canon 旧版本条目

### 7.3 段级压缩（自动 TLDR）

| 触发 | 操作 |
|------|------|
| 章节摘要 > 500 字 | 自动 TLDR（保留：关键事件 / 角色变化 / 状态结尾，3 项各 1-2 句） |
| 角色卡 > 300 字 | 提取标签 + 当前状态 + 最近一次变化 |
| Canon 条目 > 200 字 | 摘要前置 + 详细折叠 |

### 7.4 跨章合并

| 触发 | 操作 |
|------|------|
| 多个 chapter-summary 加载（≥ 5 章） | 取近 3 章 FULL + 远 N 章每章 1 行摘要 |
| memory-stream entry > 20 个 | 5 章前每 5 章合并为"阶段摘要" entry |

---

## 八、与既有体系的兼容

- 35 层物理文件**保持不变**，仅在文件头加可选 frontmatter：

```yaml
---
tier: HOT  # HOT / WARM / COLD
granularity: HEAD  # FULL / HEAD / DELTA
head_window: 5  # HEAD 模式时的窗口大小（章数 / 条数）
---
```

- frontmatter 缺省时按本文件 §2.2 默认值
- A/B/C 三级分类（按"何时加载"）与 Hot/Warm/Cold（按"读多少"）正交并存
- memory-index 升级为支持本协议的 routing：

```yaml
# memory-index.md frontmatter
---
default_load: A_LEVEL  # A_LEVEL / WARM_TRIGGERED / COLD_INIT
budget_mode: Standard  # Lite / Standard / Pro
---
```

---

## 九、与 SKILL.md 协议的对应关系

| SKILL.md 段落 | 本文件位置 | 用途 |
|---------------|------------|------|
| 协议 4 执行模式分级 | §四 Token 预算 | Lite/Standard/Pro 各自预算 |
| 门 1 写前门"对齐单已读" | §六 加载序列 | Standard 加载序列即门 1 必读项 |
| 门 5 回写门"时间线回写" | §五 memory-stream 格式 | 每章追加 entry |
| 协议 6.1 自动修复 | §四.1 压缩协议 | 超预算时自动压缩 |
| 协议 3 降级执行 | §二.3 分级动态升降 | 上下文超 20 轮时降级 |

---

## 十、维护规则

- 本文件是记忆流式加载、压缩、Token 预算、Hot/Warm/Cold 分级的唯一口径
- memory-layer-index.md 与本文件 §二.2 必须保持一致（35 层名称 + 分级）
- 新增记忆层时本文件 §二.2 同步加行
- Token 预算调整需用户显式确认；自动调整记入 gate-log §执行信息"裁剪记录"
