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
| **门 1** | 反 AI 写法预设 | 角色声线矩阵 + 感官通道 + 禁用词表 | 任一未确认 | 自动加载默认禁用词表 |
| **门 1** | 地图/时间线闭环 | world-map + timeline + universe-map | 题材需要但未建 | [world-and-timeline-output-spec](./world-and-timeline-output-spec.md) §一/§二/§三 触发条件表 |
| **门 1** | 上章 pending_writeback=false | series-state | =true | 强制回上章过门 5 |
| **门 1** | 必触隐性后果未触发 | deep-memory.md §latent-consequences | must 类未在范围内触发 | 本章必须触发或显式延后 |
| **门 1** | 承诺账本超硬上限 | deep-memory.md §promise-ledger | 埋点 + 20 章未兑 | 本章强制兑现或转移 |
| **门 1** | character-engine 实例化 | 02-characters/character-engine.md | 未实例化 | C2 降级 WARN（[consistency §C2 cold-start](./consistency-enforcement-protocol.md#c2-行为一致性)）|
| **门 2** | scene_id + 四要素 | scene-engine-template | 任一字段缺 | 协议 2.1 自动从章节卡生成最小骨架 |
| **门 3** | 反 AI 13 项实时检查 | [anti-ai-thresholds §一/§二/§三](./anti-ai-thresholds.md) | 任一 FAIL | 13 项各自修复路径 |
| **门 3** | §六 形容词 7 项 | [anti-ai-thresholds §六](./anti-ai-thresholds.md#六形容词堆砌零容忍专项) | 任一阈值超 | §六.4 自动修复路径 |
| **门 3** | dedup 实时检测 | deep-memory.md §dedup-ledger | 命中重复声明 §3.2 阈值 | §3.3 自动压缩 |
| **门 3** | 字数 ≥ 4300 | 正文计数 | < 4300 且补无可补 | 异常表"字数不足"→ 回退门 1 |
| **门 4** | P0#1-#7 结构硬检 | 章节卡 + canon + timeline + 上章 | 任一 FAIL | 协议 6.1 自动修复（最多 3 次）|
| **门 4** | P0#5 结尾钩子 | 正文 + promise-ledger | 钩子缺 / 未登记 ledger | 自动登记 + 设软/硬截止 |
| **门 4** | P0#8 AI 检测 ≤ 5% | 13+7+19 = 39 项硬检全 PASS | 任一 FAIL | [anti-ai-thresholds 优先级](./anti-ai-thresholds.md#八2-判定规则) 修复 |
| **门 4** | P0#9 一致性 5 类 | canon + timeline + character-engine + deep-memory | 任一类 FAIL | [consistency §三](./consistency-enforcement-protocol.md#三自动修复路径) 修复 |
| **门 4** | §七 AI 感扫描 19 项 | 正文 + dedup-ledger | 命中 §7.2 阈值 | §7.3 P0-P4 优先级修复 |
| **门 4** | 写作意识 #10-#15 | 模型自评 | 🔴项 ≥ 3 | Auto Flow 自动修复最严重 1-2 项 |
| **门 4** | #16/#17 双轨一致 | 小说 + 剧本骨架 | 不一致 | V12-V18 校验器修复 |
| **门 5** | gate-log 已写 | 05-chapters/gate-logs/CH{N}-gate-log.md | 写入失败 | 异常表"gate-log 写入失败"→ 对话内输出 + 等用户落盘 |
| **门 5** | memory-stream 追加 entry | 07-memory/memory-stream.md | 追加失败 | 同上对话内输出 |
| **门 5** | deep-memory 8 维更新 | 07-memory/deep-memory.md | 漏更 | 漏更 → 门 5 BLOCK，重写 |
| **门 5** | pending_writeback=false | series-state | 任一回写项未完 | 必须完成才进下一章门 1 |

---

## 二、8 SSOT 横向联动矩阵

```
SKILL.md (运行时协议) ─┬→ anti-ai-thresholds (39 项计算)
                      ├→ world-and-timeline-output-spec (13 题材闭环)
                      ├→ memory-layer-index (35 层"用哪层")
                      ├→ memory-streaming-protocol (Hot/Warm/Cold + token + delta)
                      ├→ deep-memory-protocol (第 36 层 8 维 + dedup)
                      ├→ consistency-enforcement-protocol (5 类 P0#9)
                      ├→ template-index (60 模板导航)
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

---

## 三、52 项硬检完整清单（39 反 AI + 5 一致性 + 8 深度记忆）

### 反 AI 13 项（[ant-ai §一/二/三](./anti-ai-thresholds.md)）

```
表面层 8: ① 无平均句 · ② 无解释腔 · ③ 无抽象情绪堆叠 · ④ 无对白同声 ·
         ⑤ 有现场感 · ⑥ 不过度工整 · ⑦ 情绪有失控 · ⑧ 无模板感
深层层 4: ⑨ 段落思维打破 · ⑩ 叙事链非线性 · ⑪ 词汇去高频 · ⑫ 信息密度波动
纹理层 1: ⑬ 纹理验收（5 子项）
```

### §六 形容词 7 项（[ant-ai §六](./anti-ai-thresholds.md#六形容词堆砌零容忍专项)）

```
7.1.1 同名词修饰 ≤2 · 7.1.2 千字形容词 ≤30 · 7.1.3 类比修辞 ≤3/千字 ·
7.1.4 程度副词 ≤2/千字 · 7.1.5 "XX的"链长 ≤1 ·
7.1.6 同段重复修饰 ≤1 · 7.1.7 形容词起首段 = 0
```

### §七 AI 感扫描 19 项（[ant-ai §七](./anti-ai-thresholds.md#七ai-感残留全维扫描门-4-完稿后)）

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

**总计**：13 + 7 + 19 + 5 + 8 = **52 项硬检 / 维度全部协议化** ✅

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
Canon 缺失 / pending_writeback=true / AI 检测器不可用 / 字数不足 / 字数超出 /
Scene 字段缺失 / 自动修复触顶 / 反 AI 项无证据 / 用户要求绕过 /
题材主角切换 / 上下文 > 30 轮 / gate-log 写入失败
```
**全部 12 项**在 SKILL.md §异常 Fallback 表有显式处理动作 ✅

---

## 五、验证：所有"BLOCK"路径必有"修复或 Fallback"出口

```
扫描全仓 BLOCK 触发条件 = 31 处
扫描全仓 修复路径 / Fallback = 31 处一一对应

✅ 无任何 BLOCK 路径无出口
✅ 无任何 Fallback 路径无触发条件
✅ 无任何协议引用未定义的数据源
✅ 无任何编号冲突（OL-2 已修，双轨改 #16/#17）
```

---

## 六、维护规则

- 任何新增协议必须在本文件 §一 主矩阵加行
- 任何新增 SSOT 必须在 §二 联动矩阵更新
- 任何新增硬检项必须在 §三 总数同步
- BLOCK 触发条件 / Fallback 改动必须双向同步本文件 §五
- 本文件是 skill 完整闭环的唯一证明文档；与各 SSOT 不一致时优先修各 SSOT 后再同步本文件
