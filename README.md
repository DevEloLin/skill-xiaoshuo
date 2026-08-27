# skill-xiaoshuo

**写作执行协议系统**（Protocol-Based Writing System）。

中文长篇小说 / 网文连载 / 剧本的**分阶段写作工作台**。用户输入一个想法 → 系统自动生成完整结构 → 强制进入六道门执行流程 → 自动阻断低质量输出。

**不是写作提示词集合**，而是一个具备点子自动建模、强制流程控制、可验证执行、状态闭环、读者体验导向的中文小说写作操作系统。

> **权威入口**：只以 [SKILL.md](./SKILL.md) 为运行时规则。`SKILL_back.md` 与 `SKILL_v2_backup.md` 是历史对照，不可作为指令或模板来源。

---

## 三条铁律

| 铁律 | 含义 |
|------|------|
| **强制执行** Mandatory | 所有规则都是协议，不是建议。模型不能选择是否执行 |
| **可验证执行** Observable | 每道门必须输出判定 + 证据，不接受无证据的"通过" |
| **不可绕过** Non-bypassable | 用户不能要求跳过流程，门未通过则禁止继续 |

**自主执行原则**：用户输入 → 自动结构化 → 自动执行六道门 → 自动修复错误 → 自动阻断低质量输出。
默认 **Auto Flow**（全自动），用户可随时切换 **Manual Checkpoint**（手动确认）。

---

## 文档导航（先看这里 → 不必读完所有文件）

| 你想 | 读哪份 |
|------|------|
| **了解项目是什么 / 整体架构** | 本 README（继续往下读） |
| **直接开干 / 18 种模式提问示例 / 操作落地** | [USAGE.md](./USAGE.md) |
| **看模型加载时实际遵循的运行时协议** | [SKILL.md](./SKILL.md)（每次调用必读） |
| **查完整规则：41 条设计原则 / V1-V18 校验器 / 协议详细** | [SKILL-reference.md](./SKILL-reference.md)（按需加载） |
| **11 个 SSOT 索引** | 见下方 §11 SSOT 网络 |
| **看示例项目** | [examples/](./examples/) |

---

## 系统架构

### 六道硬门

```
用户输入
   ↓
[门 0] Idea 解析   →  项目简报+主角+冲突+世界约束+骨架（仅无结构化输入触发）
[门 1] 写前门      →  Canon/时间线/对齐已读 · 反AI预设 · 地图/时间线闭环
[门 2] 场景门      →  scene_id + 四要素 + 反预期点
[门 3] 正文门      →  实时自然度诊断 + dedup（篇幅服从章节功能与用户/平台要求）
[门 4] 质检门      →  P0#1-#9 硬检 + §六/§七 + 写作意识 #10-#15
[门 5] 回写门      →  gate-log + memory-stream + deep-memory 8 维更新
   ↓
pending_writeback=false → 下一章门 1
```

任一门 BLOCK → 协议 6.1 自动修复（同门 1 次 / 全程 3 次）→ 仍 BLOCK → 输出报告等用户决策。

### 七项协议

| 编号 | 协议名称 | 作用 |
|------|---------|------|
| 协议 1 | 带证据过门 | 每道门必须附带证据，无证据的"通过"等同假执行 |
| 协议 2 | 反绕过声明（含 2.1 入口锁） | 禁止跳过、合并、推迟任何门检查 |
| 协议 3 | 降级执行 | 规则过多时的保底机制，保证核心门控不丢 |
| 协议 4 | 执行模式分级 | Lite / Standard / Pro / Q 四模式自动选择 |
| 协议 5 | 执行日志（含 5.1 显式 Checklist 输出） | gate-log 写入独立文件 |
| 协议 6 | 硬拒绝格式 | 门未通过时的标准拒绝输出格式 |
| 协议 6.1 | 自动修复协议 | BLOCK 后自动修复（同门 1 次 / 全流程 3 次）|

### 四级执行模式

| 模式 | 适用 | 门控范围 | A 级 token 预算 |
|------|------|---------|---------------:|
| **Lite** | 单章试写 / 短篇 / 片段 | 门 0（如需）+ 门 1（简）+ 门 3 + 门 5（简） | 2000 |
| **Standard** | 长篇续写 / 常规连载（默认） | 6 道门全显式 | 5000 |
| **Pro** | 双轨输出 / 投稿 / 群像大布局 | 6 道门 + 完整证据 + V1-V18 全跑 | 10000 |
| **Q（日更）** | 一句方向指令 → 自动续写 | 自动 6 道门 | 1500 |

### 39 项叙事自然度诊断

```
表面层 8 + 深层 4 + 纹理 1 = 13 项（句长变化 / 解释腔 / 抽象情绪 / 对白同声 ...）
§六 形容词零容忍 7 项（同名词修饰 ≤2 / 千字形容词 ≤30 / "XX的"链 ≤1 ...）
§七 AI 感扫描 19 项（句法 / 词汇 / 段落 / 修辞 / 全局五维信号）
─────────────────────────────────────
用于定位句法、词汇、段落、修辞与全局层面的可读性病灶；量化项是编辑信号，不是外部检测器的通过承诺。发现可定位且伤害阅读的病灶时，按修复路径定向处理。
```

### 5 类一致性零容忍硬检（门 4 P0#9）

| 类别 | 检测对象 | BLOCK 条件 |
|------|---------|-----------|
| **C1 设定** | 人名 / 地名 / 道具 / 能力 / 世界规则 | 名称漂移 / 属性冲突 |
| **C2 行为** | 角色行动逻辑 / 性格底色 | 与动机相反 / 性格突变 / 关系跳跃 |
| **C3 知识** | 谁在何时知道什么 | 已知节点又表震惊 / 未知节点用作判断 |
| **C4 时空** | 时间序 / 地理位置 / 距离 / 流速 | 时间倒流 / 不可能距离 / 流速矛盾 |
| **C5 力量** | 等级 / 技能 / 装备 / 战力 | 境界跳跃 / 凭空技能 / 战力波动 |

### 36 层记忆体系

```
35 事实层（A 级 7 + B 级 17 + C 级 11）
+
1 语义层（深度记忆，含 8 维：主题母题 / 关系弧光 / 知识状态图谱 /
          承诺账本 / 读者体验指纹 / 风格指纹 / 隐性后果 / Reference-once dedup ledger）
```

两级正交分级：
- **A/B/C**（按"何时加载"）
- **🔥HOT / 🌡️WARM / 🧊COLD**（按"读多少"）

**记忆流（Memory Stream）**：每章门 5 回写后追加 entry 到 `07-memory/memory-stream.md`，含 6 类事实 delta + 1 类 deep delta；阴谋局网与并发布局按需追加 `conspiracy`、`weave` 字段。续写时优先读 stream 最近 5 章 entry。

---

## 11 SSOT 网络

| SSOT | 职责 |
|------|------|
| [`anti-ai-thresholds.md`](./references/anti-ai-thresholds.md) | 叙事自然度 39 项诊断口径（13+7+19）|
| [`world-and-timeline-output-spec.md`](./references/world-and-timeline-output-spec.md) | 世界地图 / 宇宙地图 / 时间线 输出约定（13 题材 × 3 维度闭环） |
| [`memory-streaming-protocol.md`](./references/memory-streaming-protocol.md) | 记忆流式（Hot/Warm/Cold + Token 预算 + DELTA + Memory Stream + 压缩） |
| [`memory-layer-index.md`](./references/memory-layer-index.md) | 35 层记忆"用哪层 / 加载顺序 / 项目类型必读包" |
| [`consistency-enforcement-protocol.md`](./references/consistency-enforcement-protocol.md) | 5 类一致性硬检（设定 / 行为 / 知识 / 时空 / 力量，门 4 P0#9） |
| [`deep-memory-protocol.md`](./references/deep-memory-protocol.md) | 第 36 层深度记忆 8 维 + Reference-once dedup |
| [`character-identity-tree-protocol.md`](./references/character-identity-tree-protocol.md) | 角色身份树、群像独立线、称呼边界与命名账本 |
| [`concurrent-plot-weave-protocol.md`](./references/concurrent-plot-weave-protocol.md) | 并发伏笔、多局互牵、候选人筛选与现实代价 |
| [`template-index.md`](./references/template-index.md) | 63 模板"用哪个 / 何时用 / 重叠关系" |
| [`closure-map.md`](./references/closure-map.md) | 全协议闭环验证矩阵（6 道门 × 协议 × 数据源 × BLOCK × Fallback）|
| [`gate-log-template.md`](./assets/gate-log-template.md) | gate-log 输出格式（结构/一致性硬检、自然度诊断、写作意识、6 项门 5 回写及条件 delta）|

---

## 适用场景

- 长篇小说（11-30 章单卷 / 多卷）
- 网文连载（每日续写 / 周更，强追更感）
- 设定维护（Canon / 时间线 / 伏笔 / 群像）
- 题材特化（玄幻 / 权谋 / 悬疑 / 言情 / 克苏鲁 / 谍战 / 古装 / 末世 等 13 题材）
- 风格统一（跨章风格指纹 + 章首轮换）
- 连续性审校（5 类一致性硬检）
- 剧本双轨输出（Scene Engine 同时输出小说 + 标准剧本格式）
- 平台投稿前的叙事自然度诊断与定向润色
- 日更一句话续写（Q 模式）

## 不适用场景

- 一次性整本生成（违反分阶段原则）
- 完全无约束的发散性创作
- 实时灵感记录 / 个人日记
- 短诗 / 散文 / 非叙事文体

---

## 快速启动 3 步

### 第 1 步：告诉 AI 你要写什么

> 一句话就够：「一个能看到别人死亡前 10 分钟的女主」

系统自动进入门 0（Idea 解析），生成项目简报、主角设计、世界约束、三幕结构、第 1 章章节卡和场景骨架，引导你确认后进入正文流程。

### 第 2 步：跟着流程走

AI 按 立项 → 设定 → 总纲 → 章节卡 → 场景骨架 → 正文 → 质检 → 回写 顺序引导。每一步都有对应模板，不需提前知道模板名称。

### 第 3 步：写完一章后回写状态

每章完成后系统自动经过质检门 + 回写门，更新 memory-stream + deep-memory + Canon + 时间线 + 伏笔，保证下一章能正确续接。

详细操作 / 18 种模式提问示例 / Memory Stream 实操 → 见 [USAGE.md](./USAGE.md)。

---

## 设计原则（41 条全文）

完整设计原则见 [SKILL-reference.md §核心设计原则](./SKILL-reference.md#核心设计原则41-条)。

---

## 项目目录结构（推荐）

```
novel-project/
├── 00-brief/         项目简报 + 承诺与边界
├── 01-world/         世界观 / 宇宙学 / 世界规则 / 地图 / 宇宙地图
├── 02-characters/    人物卡 / 群像总表 / 关系图 / 弧光矩阵
├── 03-outline/       总纲 / 故事线 / 结构网 / 伏笔矩阵 / 势力棋盘
├── 04-volumes/       卷纲（每卷 1 份）
├── 05-chapters/
│   ├── chapter-cards/      章节卡（每章 1 份）
│   ├── detailed-outlines/  细纲
│   ├── chapter-summaries/  章节摘要
│   └── gate-logs/          每章 gate-log（CH{N}-gate-log.md）
├── 06-drafts/        正文（每章 1 份）
├── 07-memory/        ← 关键
│   ├── memory-index.md           记忆索引（入口）
│   ├── canon-facts.md            Canon 事实表
│   ├── timeline.md               时间线
│   ├── consistency-ledger.md     连续性台账
│   ├── memory-stream.md          🆕 流式记忆（每章 delta）
│   └── deep-memory.md            🆕 第 36 层深度记忆 8 维
├── 08-review/        审校产物
└── 09-archive/       废案 / 已退役 retcon
```

---

## 质量与反幻觉

正文输出前必须通过门 4：

- **🔒 P0 硬检（FAIL 即 BLOCK）**：#1 目标 · #2 冲突 · #3 角色 · #4 无新设定 · #5 结尾钩子（仅新承诺登记 promise-ledger）· #6 映射 · #7 Canon/时间线 · #9 一致性 5 类 · #16/#17 双轨
- **📝 叙事自然度 39 项诊断**（13 + 7 + 19）：定位有害病灶后定向修复，不承诺外部检测器分数
- **📝 写作意识 P0#10-#15**：节奏 / 爽点 / 弃书 / 金句 / 梗 / 标签（自评供审阅）

反幻觉策略：先提取事实 → 不擅自新增重大 canon → 信息不足时保守输出 → Reference-once dedup 防重复声明。详见 [hallucination-control.md](./references/hallucination-control.md) 与 [closure-map.md](./references/closure-map.md)。

---

## 相关文件

- [SKILL.md](./SKILL.md) — 精简运行时版（~230 行），每次调用加载
- [SKILL-reference.md](./SKILL-reference.md) — 完整参考手册（~2600 行），按需加载
- [USAGE.md](./USAGE.md) — 操作手册 + 18 种模式提问示例
- [references/](./references/) — 11 SSOT + 33 专题参考
- [assets/](./assets/) — 63 套模板（按 [template-index.md](./references/template-index.md) 选用）
- [examples/](./examples/) — 10 个示例项目

---

## 一句话总结

**输入一句话 → 自动建模 → 强制 6 道门 → 39 项反 AI + 5 类一致性 + 36 层记忆 + Reference-once dedup → 不写错、不漂移、不重复 → 持续可续写的长篇连载工作台。**
