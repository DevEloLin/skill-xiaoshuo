# 全协议闭环验证矩阵（Closure Map · SSOT）

> **目的**：单页证明 skill-xiaoshuo 所有协议、数据源、BLOCK 条件、Fallback 形成完整闭环。
> 任何"协议 X 触发条件未明 / 数据源缺失 / BLOCK 后无 fallback"在本文件应可一行查到。
> 本文件是验证文档，不引入新规则；规则定义见各 SSOT。

---

## 一、6 道门 × 协议 × 数据源 × BLOCK × Fallback 主矩阵

| 门 | 协议 / 检查 | 数据源 | BLOCK 条件 | Fallback |
|----|-----------|--------|-----------|---------|
| **门 0** | Idea 解析 | 用户一句话输入 | 输出未含 主角+冲突+世界约束+骨架 | 协议 2.1 入口锁自动建最小骨架 |
| **门 1** | Canon/时间线/对齐单已读 | canon-facts / timeline / chapter-alignment | 任一未读 | [SKILL.md 异常表](../SKILL.md#异常与-fallback) Canon 缺失 → 触发门 0 |
| **门 1** | 价值声明已填 | 章节卡 | 价值声明空 | 自动从章节卡提取 |
| **门 1** | 自然度写法预设 | 角色声线矩阵 + 场景感官通道 | 无写前判断 | 记录预设；不以禁用词表代替编辑判断 |
| **门 1** | 角色身份树/称呼边界（触发时） | character-identity-tree + character-engine | 本章重要人物的私人线或称呼边界未读 | 加载对应 HEAD 后再写 |
| **门 1** | 并发布局线程板（触发时） | concurrent-plot-weave + conspiracy/foreshadow | 活跃多线程的关系、位置或代价未读 | 先加载 7b 线程板，再写场景 |
| **门 1** | 地图/时间线闭环 | world-map + timeline + universe-map | 题材需要但未建 | [world-and-timeline-output-spec](./world-and-timeline-output-spec.md) §一/§二/§三 触发条件表 |
| **门 1** | 上章 pending_writeback=false | series-state | =true | 强制回上章过门 5 |
| **门 1** | 必触隐性后果未触发 | deep-memory.md §latent-consequences | must 类未在范围内触发 | 本章必须触发或显式延后 |
| **门 1** | 承诺账本超硬上限 | deep-memory.md §promise-ledger | 埋点 + 20 章未兑 | 本章强制兑现或转移 |
| **门 1** | character-engine 实例化 | 02-characters/character-engine.md | 未实例化 | C2 降级 WARN（[consistency §C2 cold-start](./consistency-enforcement-protocol.md#c2-行为一致性)）|
| **门 2** | scene_id + 四要素 | scene-engine-template | 任一字段缺 | 协议 2.1 自动从章节卡生成最小骨架 |
| **门 3** | 自然度 13 项实时诊断 | [anti-ai-thresholds §一/§二/§三](./anti-ai-thresholds.md) | 病灶可定位且伤害阅读 | 定向修复对应段落 |
| **门 3** | §六形容词与修辞诊断 | [anti-ai-thresholds §六](./anti-ai-thresholds.md#六形容词堆砌零容忍专项) | 修辞妨碍动作、声线或准确性 | §六诊断与修复路径 |
| **门 3** | dedup 实时检测 | deep-memory.md §dedup-ledger | 命中重复声明 §3.2 阈值 | §3.3 自动压缩 |
| **门 3** | 篇幅与章节功能相称 | 正文 + 章节卡 + 用户/平台要求 | 灌水、过薄或违背明确交付要求 | 补真实事件、拆分或压缩，不灌水 |
| **门 4** | P0#1-#7 结构硬检 | 章节卡 + canon + timeline + 上章 | 任一 FAIL | 协议 6.1：当前门自动修复 1 次，全流程累计最多 3 次 |
| **门 4** | P0#5 结尾余波/问题/静止 | 正文 + promise-ledger（仅新承诺） | 结尾与章节目的不相称，或新承诺未登记 | 调整收束；新承诺才登记账本 |
| **门 4** | 全文自然度复查 | 13+7+19 项诊断 + 正文证据 | 有害病灶可定位 | [anti-ai-thresholds](./anti-ai-thresholds.md) 定向修复 |
| **门 4** | P0#9 一致性 5 类 | canon + timeline + character-engine + deep-memory | 任一类 FAIL | [consistency §三](./consistency-enforcement-protocol.md#三自动修复路径) 修复 |
| **门 4** | §七 AI 感扫描 19 项 | 正文 + dedup-ledger | 命中 §7.2 阈值 | §7.3 P0-P4 优先级修复 |
| **门 4** | 写作意识 #10-#15 | 模型自评 | 🔴项 ≥ 3 | Auto Flow 自动修复最严重 1-2 项 |
| **门 4** | #16/#17 双轨一致 | 小说 + 剧本骨架 | 不一致 | V12-V18 校验器修复 |
| **门 4** | 角色身份树与命名审校（触发时） | identity-tree + relationship/knowledge state | 重要角色可互换、称呼越权或事件线无故失联 | 回退角色卡/场景目标修复；C2/C3 冲突仍 BLOCK |
| **门 4** | 并发布局审校（触发时） | concurrent-plot-weave + foreshadow/conspiracy matrix | 并发线互相矛盾、遮蔽靠新增规则，或底层人物只作耗材 | 回退线程板，补足因果、反制或现实后果；仍冲突则 BLOCK |
| **门 5** | gate-log 已交付 | 用户指定路径的 gate-log，或对话内 Checklist | 用户已授权落盘但写入失败 | 对话内输出完整 Checklist + 等用户落盘；未授权写入时对话交付即通过 |
| **门 5** | memory-stream 追加 entry / 状态交接包 | 已授权时 `07-memory/memory-stream.md`；未授权时对话交接包 | 已授权写入失败，或交接包字段不完整 | 对话内输出完整交接包；下轮必须提供交接包或状态文件 |
| **门 5** | deep-memory 8 维更新 / 交接包 deep | 已授权时 `07-memory/deep-memory.md`；未授权时对话交接包 | 8 维漏更 | 补齐文件或交接包的 8 维字段后重验 |
| **门 5** | pending_writeback=false / complete-in-chat | 已授权时 series-state；未授权时交接包 `writeback` | 任一回写项未完 | 补齐回写；未授权模式下，下轮先索取交接包 |

---

## 二、11 个 SSOT 横向联动矩阵

```
SKILL.md (运行时协议) ─┬→ anti-ai-thresholds (39 项自然度诊断)
                      ├→ world-and-timeline-output-spec (13 题材闭环)
                      ├→ memory-layer-index (35 层"用哪层")
                      ├→ memory-streaming-protocol (Hot/Warm/Cold + token + delta)
                      ├→ deep-memory-protocol (第 36 层 8 维 + dedup)
                      ├→ consistency-enforcement-protocol (5 类 P0#9)
                      ├→ character-identity-tree-protocol（角色树、私人线与命名）
                      ├→ concurrent-plot-weave-protocol（并发局、培育筛选与多线程伏笔）
                      ├→ template-index (63 模板导航)
                      ├→ gate-log-template (输出格式)
                      └→ closure-map (本文件 / 验证)
SKILL-reference.md ─→ 完整规则参考（按需加载）
README.md / USAGE.md ─→ 项目门面 + 操作手册
```

### 协议间数据流（双向闭环）

| 协议 A | ↔ | 协议 B | 数据流方向 |
|--------|:-:|--------|-----------|
| canon-facts | ↔ | timeline | 互相比对（事件↔Canon）|
| world-map | ↔ | timeline | 事件→地点绑定（双向校验）|
| world-map | ↔ | universe-map | 嵌套 + 通路入口（双向校验）|
| canon-facts | ↔ | dedup-ledger | dedup 简称必可解析回 canon |
| timeline | ↔ | knowledge-state | 知识变更必有 timeline 事件支撑 |
| timeline | ↔ | latent-consequences | must 类后果必有起因事件 |
| chapter-summary | ↔ | memory-stream | summary 是 stream entry 的事实级版 |
| gate-log | ↔ | memory-stream | gate-log §记忆流 delta 是 stream entry 的草稿 |
| character-engine | ↔ | relations-arcs | 角色动机 + 关系阶段联合判 C2 |
| promise-ledger | ↔ | P0#5 结尾钩子 | 钩子必登记 ledger |
| concurrent-plot-weave | ↔ | conspiracy/foreshadow/identity tree | J 局、F 伏笔、人物位置与 R 代价在同一场景可并发校验 |

---

## 三、诊断与连续性维度索引（39 自然度 + 5 一致性 + 8 深度记忆）

### 自然度 13 项（[自然度 §一/二/三](./anti-ai-thresholds.md)）

```
表面层 8: ① 无平均句 · ② 无解释腔 · ③ 无抽象情绪堆叠 · ④ 无对白同声 ·
         ⑤ 有现场感 · ⑥ 不过度工整 · ⑦ 情绪有失控 · ⑧ 无模板感
深层层 4: ⑨ 段落思维打破 · ⑩ 叙事链非线性 · ⑪ 词汇去高频 · ⑫ 信息密度波动
纹理层 1: ⑬ 纹理验收（5 子项）
```

### §六 形容词与修辞 7 项（[自然度 §六](./anti-ai-thresholds.md#六形容词堆砌零容忍专项)）

```
7.1.1 同名词修饰 ≤2 · 7.1.2 千字形容词 ≤30 · 7.1.3 类比修辞 ≤3/千字 ·
7.1.4 程度副词 ≤2/千字 · 7.1.5 "XX的"链长 ≤1 ·
7.1.6 同段重复修饰 ≤1 · 7.1.7 形容词起首段 = 0
```

### §七 模板感扫描 19 项（[自然度 §七](./anti-ai-thresholds.md#七ai-感残留全维扫描门-4-完稿后)）

```
句法 6 + 词汇 4 + 段落 3 + 修辞 3 + 全局 3 = 19 项（详见 §七）
```

### 一致性 5 类（[consistency §一](./consistency-enforcement-protocol.md)）

```
C1 设定 · C2 行为 · C3 知识 · C4 时空 · C5 力量
```

### 深度记忆 8 维（[deep-memory §二](./deep-memory-protocol.md)）

```
1 motifs · 2 relations-arcs · 3 knowledge-state · 4 promise-ledger ·
5 reader-fingerprint · 6 style-fingerprint · 7 latent-consequences · 8 dedup-ledger
```

**说明**：39 项用于定位自然度病灶；C1-C5 为连续性硬检；8 个深度记忆维度用于回写与追踪。它们不应被误写为“52 项都一律 BLOCK”。

---

## 四、Cold-start / 裁剪模式 / 异常 Fallback 全覆盖证明

### 4.1 Cold-start（项目早期）

| 协议 | Cold-start 触发条件 | Fallback 行为 |
|------|-------------------|--------------|
| deep-memory 8 维 | 项目章数 ≤ 3 | [§4.1.1](./deep-memory-protocol.md#411-cold-start-fallback前-3-章项目启动时) cold-start 表（仅 build 不 check）|
| C2 行为一致性 | character-engine 未实例化 | [C2 cold-start](./consistency-enforcement-protocol.md#c2-行为一致性) 降级 WARN |
| timeline | 第 1 章无历史 | spec §3.1 触发条件 = 任何项目必建（1 章起即开始）|
| memory-stream | 第 1 章 stream 文件不存在 | streaming §5.1 自动建 |
| 风格指纹 | 章数 < 5 无 baseline | deep-memory §4.1.1 表第 6 行：CH6 起 check |

### 4.2 裁剪模式（>30 轮）

| 协议 | 裁剪后状态 | Fallback |
|------|-----------|---------|
| A 级 7 层 | 全保留 | 不变 |
| B 级 17 层 + WARM | 不加载 | streaming §四.1 自动压缩 |
| C 级 11 层 + COLD | 不加载 | 缓存 + 仅初始化加载 |
| C2 / C5 一致性 | character-engine 缺失 | [consistency §6.1](./consistency-enforcement-protocol.md#61-裁剪模式-fallback上下文--30-轮强制裁剪时) 降级 WARN |
| C1/C3/C4 | A 级数据源都在 | 仍 BLOCK 级 |

### 4.3 12 项异常（[SKILL.md 异常 Fallback 表](../SKILL.md#异常与-fallback)）

```
Canon 缺失 / pending_writeback=true / 外部检测报告不可用或异常 / 篇幅与章节功能不匹配 /
Scene 字段缺失 / 自动修复触顶 / 自然度诊断无证据 / 用户要求绕过 /
题材主角切换 / 上下文 > 30 轮 / gate-log 写入失败
```
**全部 12 项**在 SKILL.md §异常 Fallback 表有显式处理动作 ✅

---

## 五、验证：所有"BLOCK"路径必有"修复或 Fallback"出口

```
每次发布前，重新扫描全部 BLOCK 触发条件及其修复 / Fallback 出口，并同步本节的结果。

本次新增的并发布局检查已有门 1 读取出口与门 4 回退出口；其余断言须以实际校验结果为准，不能用历史计数替代验证。
```

---

## 六、维护规则

- 任何新增协议必须在本文件 §一 主矩阵加行
- 任何新增 SSOT 必须在 §二 联动矩阵更新
- 任何新增硬检项必须在 §三 总数同步
- BLOCK 触发条件 / Fallback 改动必须双向同步本文件 §五
- 本文件是 skill 完整闭环的唯一证明文档；与各 SSOT 不一致时优先修各 SSOT 后再同步本文件
