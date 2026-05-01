# 记忆层索引（35 层 × 加载触发 × 模板映射）

> 这份索引解决三个问题：
> 1. **每层记忆对应哪个 assets 模板 / 哪份 references 设计文档**
> 2. **不同项目类型分别要建哪些层**（短篇 vs 长篇 vs 群像 vs 多卷）
> 3. **加载顺序与触发条件**

详细规则定义见 [SKILL-reference.md §记忆体系完整 35 层](../SKILL-reference.md#记忆体系完整-35-层)。本文件只做"哪一层 / 用什么 / 何时建"的导航。

> **配套**：[memory-streaming-protocol.md](./memory-streaming-protocol.md) 给"哪层读多少 / Token 预算 / Hot-Warm-Cold 分级 / Memory Stream / 压缩协议"，与本文件正交（A/B/C 三级 = "何时加载"；HOT/WARM/COLD = "读多少"）。

---

## 一、35 层全表（带模板映射）

### A 级：常驻核心 7 层（每次长篇写作必读）

| 层 # | 名称 | 用途 | 对应模板 | 对应 references |
|-----:|------|------|---------|----------------|
| 10 | 记忆索引 | 入口，规定本轮读什么 | `assets/memory-index-template.md` | — |
| 1 | Canon 事实表 | 不可改的设定（硬约束） | `assets/canon-facts-template.md` | `hallucination-control.md` |
| 3 | 时间线 | 事件顺序（硬约束） | `assets/timeline-template.md` | — |
| 2 | 人物状态表 | 角色当前处境 / 关系 / 心理 | （含在 character / ensemble 模板内） | `character-engine-design.md` |
| 5 | 章节摘要 | 续写前快速回忆 | `assets/chapter-summary-template.md` | — |
| 28 | 章节对齐单 | 前后章承接 | `assets/chapter-alignment-template.md` | — |
| 29 | 十章呼应板 | 最近 10 章未结线 | `assets/ten-chapter-echo-template.md` | — |

### B 级：条件加载 17 层（按当前任务加载）

| 层 # | 名称 | 加载条件 | 对应模板 | 对应 references |
|-----:|------|---------|---------|----------------|
| 4 | 伏笔与线索表 | 需要埋线 / 回收时 | `assets/foreshadow-matrix-template.md` | — |
| 6 | 势力棋盘 | 涉及多阵营推进时 | `assets/faction-board-template.md` | — |
| 7 | 结构网图 | 大布局 / 多线叙事时 | `assets/plot-architecture-template.md` + `storyline-board-template.md` | `macro-plotting.md` |
| 11 | 连续性台账 | 审校 / 续写时 | `assets/consistency-ledger-template.md` | — |
| 12 | 群像总表 | 群像章节时 | `assets/ensemble-cast-template.md` | `ensemble-writing.md` |
| 13 | 群像弧光矩阵 | 群像弧光设计时 | `assets/ensemble-arc-matrix-template.md` | `ensemble-writing.md` |
| 14 | 未知钩子矩阵 | 悬疑推进时 | `assets/unknown-hook-matrix-template.md` | `unknown-hook-design.md` |
| 15 | 主角主动性追踪 | 主角连续被动时 | `assets/protagonist-agency-template.md` | — |
| 21 | 多阵营动态表 | 多阵营章节时 | `assets/multi-faction-dynamics-template.md` | — |
| 25 | 冲突节奏板 | 冲突密度调整时 | `assets/conflict-rhythm-template.md` | `conflict-rhythm-design.md` |
| 27 | 金句与有趣角色单 | 需要活气 / 金句时 | `assets/quotable-line-and-comic-role-template.md` + `funny-role-template.md` | `funny-role-design.md` · `quotable-line-and-comic-role-design.md` |
| 30 | 支线脆弱度表 | 支线超时预警时 | `assets/subplot-fragility-template.md` | — |
| 31 | 场景骨架表 | 进入场景拆分时 | `assets/scene-engine-template.md` | `scene-engine-design.md` · `scene-schema.md` |
| 32 | 信息流控制单 | 控制信息释放时 | `assets/information-flow-template.md` | `information-flow-control.md` |
| 33 | 节奏控制单 | 节奏设计时 | `assets/pacing-engine-template.md` | `pacing-engine-design.md` |
| 34 | 角色驱动单 | 分配驱动角色时 | （含在 chapter-card 内） | — |
| 35 | 质量闭环单 | 正文完成质检时 | `assets/quality-gate-template.md` | `quality-gate-design.md` · `quality-rubric.md` |

### C 级：专项 11 层（特定模式 / 题材才读）

| 层 # | 名称 | 加载条件 | 对应模板 | 对应 references |
|-----:|------|---------|---------|----------------|
| 8 | 宇宙设定 | 模式 H（宇宙设定）时 | `assets/cosmology-template.md` + `world-rules-template.md` | `world-logic.md` |
| 9 | 地图体系 | 涉及地理 / 跨界时 | `assets/world-map-template.md` + `universe-map-template.md` | — |
| 16 | 投稿润色单 | 模式 M（投稿）时 | `assets/submission-polish-template.md` | `platform-submission.md` |
| 17 | 去 AI 味诊断单 | 模式 N（去 AI）时 | `assets/de-ai-diagnostic-template.md` | `de-ai-rewriting-patterns.md` · `anti-ai-thresholds.md` |
| 18 | 章首轮换表 | **连载项目自动升级为每章必读** | `assets/chapter-opening-rotation-template.md` | `first-sentence-de-ai.md` |
| 19 | 章首第一句表 | 首句打磨时 | `assets/first-sentence-template.md` | `first-sentence-technique-library.md` |
| 20 | 单章字数控制单 | 字数不达标时 | `assets/chapter-length-control-template.md` | `chapter-length-enforcement.md` |
| 22 | 情绪爆点表 | 高潮 / 爆发场景时 | `assets/emotion-burst-template.md` | `raw-emotion-writing.md` |
| 23 | 对白声线矩阵 | 对白打磨时 | `assets/dialogue-voice-matrix-template.md` | `dialogue-voice-differentiation.md` |
| 24 | 情绪价值板 | 情绪设计时 | `assets/emotional-value-board-template.md` | `emotional-value-design.md` |
| 26 | 魅力角色表 | 角色设计时 | `assets/magnetic-character-template.md` | `magnetic-character-design.md` |

---

## 二、加载顺序协议

### 默认加载序列（每次写正文前）

```
1. memory-index（层 10）       ← 先看本轮读什么
   ↓
2. canon-facts（层 1）         ← 硬约束
3. timeline（层 3）            ← 硬约束
   ↓
4. consistency-ledger（层 11） ← 潜在冲突点（B 级，长篇必加）
   ↓
5. 当前卷纲 / 当前细纲 / 章节卡（含层 28）
   ↓
6. chapter-summary 最近 N 章（层 5）
7. ten-chapter-echo（层 29）   ← 防止失联
   ↓
8. 按任务追加 B / C 级层
   ↓
9. 进门 1 写前检查
```

### 关键加载规则

- **不要一次全读**：A 级 7 层是基线，B / C 级**只读当前任务相关的**
- **memory-index 是入口**：每次都从这里开始，避免重复读
- **rollback_pending=true 时**：必须重读全部 A 级 + 上次回滚版本对照，不能跳过
- **连载项目层 18 实际为每章必读**：因为门 3 #7 章首轮换强制检查

---

## 三、按项目类型选必读层

### 短篇（≤ 3 章）

最低 3 层：`memory-index` · `canon-facts` · `chapter-summary`。其余按内容补。

### 长篇连载（11 - 30 章单卷）

A 级 7 层全部 + B 级最低 5 层：

```
A 级（7）: 全部
B 级最低: 4 伏笔表 · 11 连续性台账 · 31 场景骨架 · 35 质量闭环 · 32 信息流
```

### 多卷长篇（>30 章）

在长篇基础上再加 B 级 4 层：

```
+ 7 结构网图 · 30 支线脆弱度 · 33 节奏控制 · 25 冲突节奏板
```

### 群像项目

在以上基础上加 B 级 4 层：

```
+ 12 群像总表 · 13 群像弧光矩阵 · 21 多阵营动态表 · 6 势力棋盘
```

### 宏大世界观

在以上基础上加 C 级 2 层：

```
+ 8 宇宙设定 · 9 地图体系
```

### 悬疑 / 解密

在以上基础上加 B 级 1 层：

```
+ 14 未知钩子矩阵
```

### 投稿（M 模式）/ 去 AI（N 模式）

```
+ 16 投稿润色单（M）
+ 17 去 AI 味诊断单（N）
```

### 双轨剧本（O 模式）

```
+ 31 场景骨架表（共用 scene_id 输出小说 + 剧本）
```

### 日更（Q 模式）

跑独立流程，仍读 A 级 7 层 + 章节卡，不强制 B 级。

---

## 四、重叠关系澄清

### Canon vs Continuity Ledger（层 1 vs 层 11）

| 层 | 内容 |
|----|------|
| 层 1 Canon 事实表 | **设定本身**：人名 / 能力 / 关系等不可改的事实 |
| 层 11 连续性台账 | **冲突点台账 + 修订记录**：哪些设定可能矛盾、哪一轮修了什么 |

简单说：Canon 写"是什么"，Continuity 写"哪里可能崩 + 已经处理过什么"。

### 章节级三层（层 5 / 28 / 29）

| 层 | 范围 |
|----|------|
| 层 5 章节摘要 | 单章发生了什么（每章 1 份）|
| 层 28 章节对齐单 | 前后两章的承接（写下一章前对齐）|
| 层 29 十章呼应板 | 最近 10 章范围内未结线（防止 5-10 章后失联）|

三层看的是不同时距：1 章 / 2 章 / 10 章。

### 群像三层（层 12 / 13 / 21）

| 层 | 范围 |
|----|------|
| 层 12 群像总表 | 角色清单 + 功能分层 |
| 层 13 群像弧光矩阵 | 每角色独立弧光（行=角色，列=阶段）|
| 层 21 多阵营动态表 | 反派 / 中立 / 敌对方的动作线 |

群像总表是"花名册"，弧光矩阵是"成长史"，多阵营动态是"敌我对抗动作链"。

### 字数 / 信息 / 节奏三层（层 20 / 32 / 33）

| 层 | 控制对象 |
|----|---------|
| 层 20 单章字数控制单 | 字数总量 + 各段分配 |
| 层 32 信息流控制单 | 信息释放节奏（什么信息何时给读者）|
| 层 33 节奏控制单 | 张力曲线 / 段落级变速 |

三个一起用：字数管"长度"，信息流管"内容露多少"，节奏管"读起来快慢"。

---

## 五、检索规则

1. 写新章节前：必读 A 级 7 层（按 §二 序列）
2. 找不到该用哪一层时：先看本索引 §一 加载条件列，再看 §四 重叠关系
3. 项目刚启动：按 §三 选必读包，避免一次性建 35 层文件
4. 多个层名相近时：优先看 §四 重叠澄清表

---

## 六、维护规则

- 新增记忆层时，本文件同步加行
- 层名 / 加载条件 / 模板映射变化时同步更新
- SKILL-reference.md §记忆体系 与本文件的层号 / 名称必须一致
