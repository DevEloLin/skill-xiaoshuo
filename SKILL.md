---
name: skill-xiaoshuo
description: 'Use when planning/writing/continuing/auditing/revising 中文长篇小说/网文连载/剧本：分阶段写作工作台（立项→设定→大纲→章节卡→场景→正文→质检→回写），非一次性生成器。强制 6 道门 + 39 项反AI硬检(≤5%) + 5 类一致性零容忍(设定/行为/知识/时空/力量) + 36 层记忆(含深度记忆 8 维 + Reference-once dedup) + 单章 ≥4300 字 + 13 题材世界地图/时间线闭环。覆盖续写润色、群像多线、双轨剧本、降AI味、平台投稿。Triggers: 小说, 网文, 续写, 润色, 连载, 大纲, 章节卡, 群像, 多线, 伏笔, 钩子, 时间线, 世界观, 玄幻, 权谋, 克苏鲁, 悬疑, 古装, 谍战, 起点, 知乎, 投稿, 去AI, 编剧, 剧本, 双轨, 日更, screenplay, novel writing.'
argument-hint: '说明题材 / 篇幅 / 已有素材 / 当前想做什么（新建 / 续写第 N 章 / 润色 / 降 AI 味 / 双轨剧本 / 日更）。'
user-invocable: true
disable-model-invocation: false
---

# 小说写作 Skill

> **写作执行协议系统**（Protocol-Based Writing System）。
> 规则是协议不是建议。强制执行、可验证、不可绕过。
> 先判断阶段 → 只处理当前任务 → 写入外部文件 → 下一轮读取再推进。

## 核心原则

1. **目标优先级**：可读性 > 情绪价值 > 信息推进 > 人物魅力 > 结构正确性
2. **每章基准要求**：≥1 情绪兑现点 + ≥1 信息推进点（写作意识项，Checklist 自评）
3. **带证据过门**：每道门输出判定+依据，无证据的"✅通过"等同未过门
4. **反绕过**：无骨架不写正文 · 无 scene_id 不写正文 · 交稿后不回写不开下一章
5. **自动修复**：BLOCK → 自动定位+修复+重入门检查（同一门最多 1 次，全流程最多 3 次）
6. **显式 Checklist**：所有门检查结果必须以 Checklist 形式写入 `05-chapters/gate-logs/CH{N}-gate-log.md`，不嵌入正文
7. **🚨 AI 检测硬限 ≤5%**：所有正文输出必须达到 AI 检测工具（GPTZero 等）判定 Human ≥95% 的标准。这是与硬检同级的 BLOCK 约束，贯穿门 1 到门 4 全流程。**不是修表面词汇，而是从段落思维结构、叙事链、词汇概率分布、信息密度波动四个底层维度消除 AI 统计特征，同时从纹理层注入人味——角色毛边、素段、配角真实个性、信息弯路、金句节制**。13 项硬检的[**量化计算方法、PASS/FAIL 阈值、自评与外审流程详见 ref**](./references/anti-ai-thresholds.md)（句长变化率怎么算、段长标准差怎么算、金句间隔怎么测的 SSOT）
8. **🚨 形容词堆砌零容忍 + AI 感全维扫描**：13 项之外另设两道闭门检查。**§六 形容词堆砌零容忍**——同名词修饰 ≤2 个 · 千字形容词 ≤30 · 类比修辞 ≤3/千字 · 程度副词 ≤2/千字 · 禁用 `XX的XX的` 链 · 禁段首形容词起首；删完用动词 / 物件 / 身体反应 / 环境互动替代。**§七 AI 感残留 19 项信号扫描**——句法 / 词汇 / 段落 / 修辞 / 全局五维信号（主语开头比例 / "他她"开头比例 / 排比句 / 量词单调 / 三层比喻嵌套 / 拟人化场景 / "了"字结尾比例 / "的地得"密度 / 章末金句收尾连续性等），命中即触发自动修复。两节[**详见 ref**](./references/anti-ai-thresholds.md#六形容词堆砌零容忍专项)，与 13 项联动执行，永不降级
9. **🚨 一致性零容忍**（**绝对不能出现前后不一致**）：5 类硬检 —— **C1 设定** / **C2 行为** / **C3 知识** / **C4 时空** / **C5 力量**。每章门 4 全章扫描，任一类 FAIL → BLOCK + 自动修复（最多 3 次）；触顶仍 FAIL → 输出 BLOCK 报告等用户决策。数据源 = canon-facts + timeline + character-engine + deep-memory.md（knowledge-state / relations-arcs / latent-consequences）。[**详见 ref**](./references/consistency-enforcement-protocol.md)，永不降级
10. **🚨 深度记忆 + Reference-once dedup**：35 层之外加第 36 层"深度记忆"，覆盖 **8 维语义层** —— 主题母题 / 关系弧光 / 知识状态图谱 / 承诺账本 / 读者体验指纹 / 风格指纹 / 隐性后果 / 重复声明记录。**Reference-once 原则** —— 设定 / 角色 / 物件 / 规则**首次说全后**，后续只用简称；连续 3 章重复定义即门 4 BLOCK 自动压缩。承诺账本超硬上限（埋点 + 20 章）未兑现 → 门 1 BLOCK。[**详见 ref**](./references/deep-memory-protocol.md)

## 六道门概览

门 0 → 门 1 → 门 2 → 门 3（写正文）→ 门 4 → 门 5，串行执行，不通过不继续。

```
用户输入
   ↓
[门 0] Idea 解析 → 项目简报+主角+冲突+世界约束+骨架      （仅无结构化输入触发）
   ↓
[门 1] 写前门    → Canon/时间线/对齐已读 · 反AI预设 ·    (读 A 级 7 层 + memory-stream HEAD)
                  地图/时间线闭环 · pending_writeback=false
                  必触隐性后果检 · 承诺账本超期检
   ↓
[门 2] 场景门    → scene_id + 四要素 + 反预期点          (Scene Engine SSOT)
   ↓
[门 3] 正文门    → 实时 39 项反AI(13+7+19) + dedup       (字数 ≥4300，触发即压缩)
   ↓
[门 4] 质检门    → P0#1-#9 硬检 + §六 + §七 +            (52 项硬检串行)
                  P0#9 一致性 5 类 C1-C5 +
                  写作意识 #10-#15 自评 + #16/#17 双轨
   ↓
[门 5] 回写门    → gate-log 写盘 + memory-stream 追加    (含 deep + consistency 段)
                  + deep-memory 8 维更新 + Canon/伏笔
   ↓
pending_writeback=false → 下一章门 1
```

任一门 BLOCK → [协议 6.1 自动修复](./SKILL-reference.md#协议-61自动修复协议recovery-protocol)（同门 1 次 / 全程 3 次）→ 仍 BLOCK → 输出报告等用户决策。


| 门 | 职责 | 关键检查 | 详细 |
|----|------|---------|------|
| **门 0** | Idea 解析 | 一句话→项目简报+主角+冲突+世界约束+骨架 | 仅无结构化输入时触发 |
| **门 1** | 写前门 | Canon/时间线/对齐单已读 · 纲追溯 · 角色标签 · 回写追查 · **反AI写法预设** | 11 项，[详见 ref](./SKILL-reference.md#门-1写前门进入正文前) |
| **门 2** | 场景门 | scene_id · 进出状态 · 类型不连续 · 非主角场景 · 反预期点 · **骨架反AI预检** | 8 项，[详见 ref](./SKILL-reference.md#门-2场景门场景骨架完成后) |
| **门 3** | 正文门 | 映射完整 · Canon无冲突 · 字数≥4300 · 章首 · 对话 · 群像 · **文笔质感** · **🚨反AI实时检查(含纹理层)** | 11 项，[详见 ref](./SKILL-reference.md#门-3正文门正文生成中后) |
| **门 4** | 质检门 | 硬检(P0结构核心) + **🚨AI检测≤5%硬检** + 写作意识(P0体验+好看+P1+P2) | 31 项，[详见 ref](./SKILL-reference.md#门-4质检门正文完成后-统一质量体系) |
| **门 5** | 回写门 | 摘要/时间线/Canon/伏笔/对齐单/纲更新 | 6 项，[详见 ref](./SKILL-reference.md#门-5回写门交稿后-强制闭环) |

### 门 0 最小产出

只有一句话想法/零散片段/无结构化输入时触发。必须产出：项目简报 + 主角(动机/伤口/目标) + 冲突 + 世界约束≥3条 + 总纲骨架 + 第1章章节卡 + 最小2-3场景骨架。风险未转译为约束 → BLOCK。已有结构化输入 → 跳过门 0。

### 门 1 核心要求

写正文前必须确认：场景骨架+Canon+时间线+对齐单已读 · 价值声明已填 · 纲追溯通过 · 角色标签(≥2人各具标签+表现+代价) · 上章回写已完成(pending_writeback=false 且六项内容一致) · **反AI写法预设已确认**（本章角色声线差异点≥3种、感官通道≥2种已预分配、禁用词表已加载）· **🚨地图/时间线闭环已建**（如项目类型需要 world-map 必建并通过 spec 验收；timeline 任何项目必建并通过 spec 验收；多界故事 universe-map 必建；详见 [world-and-timeline-output-spec.md](./references/world-and-timeline-output-spec.md)，漏建即 BLOCK）。纲体系按规模自动分类(≤3短篇/4-10中篇/11-30长篇单卷/>30多卷)。

### 门 4 Checklist 分级

门 4 的 31 项检查分为两类，**执行方式不同**：

**🔒 硬检（可文件对比验证，FAIL → BLOCK）**：

P0#1目标实现 · #2冲突进展 · #3角色逻辑 · #4无新设定 · **#5结尾钩子(必登记 promise-ledger，附 ID + 软/硬截止章)** · #6映射校验 · #7Canon/时间线无冲突 · **#8 🚨AI检测≤5%** · **#9 🚨一致性零容忍C1-C5** · #16/#17双轨一致(仅双轨)

**🚨 AI检测≤5% 硬检（P0#8，FAIL → BLOCK，与结构硬检同级）**：

全文必须通过以下 13 项反AI验收（8项表面+4项深层+1项纹理），任一不通过 → BLOCK → 自动修复：

**表面层（快速拦截）**：
① 无平均句（同段内句长变化率≥30%，禁止全段句子长度接近）
② 无解释腔（禁止先结论后动作，禁止作者替角色总结心理）
③ 无抽象情绪词堆叠（"震惊/愤怒/绝望"必须替换为身体反应/动作/环境细节）
④ 无对白同声（≥3角色声线可辨，遮名后能猜出谁说的）
⑤ 有现场感（每个场景必须有可见动作+环境参与，禁止回顾梗概式写法）
⑥ 不过度工整（保留粗粝感/未说满处/情绪毛边，禁止每段都圆满收束）
⑦ 情绪有失控（高压段允许短句/断句/不完整句/嘴硬/失手，禁止全程理性克制）
⑧ 无模板感（章首/段首/收束句法多样，连续3段不得使用相同句式结构）

**深层结构（消除AI统计特征）**：
⑨ 段落思维打破（≥30%段落无结论句 · 段长标准差≥段均长40% · 禁止段段扣题 · 有中途转向段）
⑩ 叙事链非线性（≥2处因果断裂/延迟 · ≥1处反应偏移 · ≥1处信息故意遗漏）
⑪ 词汇去高频化（禁用"缓缓/微微/轻轻/淡淡" · 动词去安全化 · 每800字≥1处口语碎片 · 连接词不重复）
⑫ 信息密度波动（同时存在密段和空段 · ≥1处大事小写 · 解释不超2段连续 · 释放方式≥3种）

**纹理层（消除AI完美感）**：
⑬ 纹理层验收（≥2处角色毛边反应[卡壳/说错/否认/废话] · ≥20%段落为素段[零修辞] · ≥1配角有不推进主线的真实个性 · ≥1处信息揭示不顺[理解错/试错/弯路] · 金句≤4句且间隔≥800字，禁止连续2段有设计感明显的句子）

**🚨 P0#9 一致性零容忍硬检（FAIL → BLOCK，5 类全扫）**：

**C1 设定** · **C2 行为** · **C3 知识** · **C4 时空** · **C5 力量**。任一类 FAIL → 自动修复（最多 3 次）→ 仍 FAIL → BLOCK 报告等用户决策。数据源 = canon-facts + timeline + character-engine + deep-memory.md（knowledge-state / relations-arcs / latent-consequences）。**绝对不能出现前后不一致**。详见 [consistency-enforcement-protocol.md](./references/consistency-enforcement-protocol.md)。

**📝 文笔质感（门 3 #10 强制 + 门 4 P1#11 二次验收）**：

无形容词堆砌 · 细节代替概括 · 多感官(≥2种) · 侧面描写存在 · 细节有功能(感官承载情绪/象征承载主题)

**📝 写作意识（自我评判项，输出评估+标注，不自动BLOCK）**：

P0#10节奏变化 · #11爽点+推进 · #12弃书风险 · #13金句≥2 · #14梗≥1+1 · #15标签体现 · P1全11项 · P2全5项

> 写作意识项的评估结果输出到 Checklist 中供用户审阅。P1 中 ≥3 项🔴 → Auto Flow 下自动修复最严重的 1-2 项；其余等用户确认后下一轮修复。

## 执行日志与 Checklist 输出协议

执行日志和 Checklist **不嵌入正文**，单独写入 `05-chapters/gate-logs/CH{N}-gate-log.md`。正文输出保持干净。

**模板** = [`assets/gate-log-template.md`](./assets/gate-log-template.md)（46 项硬检 + 22 项写作意识 + 6 项门 5 回写 + 6 类记忆流 delta 字段）。

**输出位置**：项目根 `05-chapters/gate-logs/CH{N}-gate-log.md`；用户未指定项目目录则在 cwd 下创建。

**规则**：
- 硬检任一 FAIL → 正文不交付，自动修复后重输出
- 写作意识项 🟢/🟡/🔴 自评 + 证据，输出供用户审阅
- 用户看到 🔴 可要求修复，下一轮执行
- 门 5 回写清单 Auto Flow 下自动补完
- 正文交付时简提 "gate-log 已写入 ..."
- 门 5 回写后追加 "记忆流 delta" 段（详见 [memory-streaming-protocol.md](./references/memory-streaming-protocol.md)）


## 执行模式

| 模式 | 触发 | 门检查 | Checklist |
|------|------|--------|-----------|
| **Lite** | 单章试写/短篇/片段 | 门0(如需)+门1(简)+门3(核心)+门5(简) | 硬检前4项 + 写作意识缩减 |
| **Standard** | 长篇续写/常规连载 | 全部6道门 | 完整 Checklist |
| **Pro** | 双轨/群像/投稿润色 | 全部6道门+完整证据 | 完整 Checklist + P2 详细 |

降级：Pro→Standard(>15轮)，Standard→Lite(>20轮)。

**任务模式映射**：P Idea / A 新建 / B 续写 / C 润色 / D 审校 / E 连载 / F 题材 / G 大布局 / H 宇宙设定 / I 写法吸收 / J 整理 / K 群像 / L 未知钩子 / M 投稿 / N 去AI / O 剧本 / Q 日更续写。默认模式和降级规则[详见 ref](./SKILL-reference.md#协议-4执行模式分级)。

## 降级与裁剪

| 正常 | 降级 | 绝对底线 |
|------|------|---------|
| 6道门全部显式 | 门0简化+门2/4可简写 | 门1/5不可跳 |
| P0全查+P1+P2 | P0硬检全查+写作意识缩减 | P0硬检前7项+🚨AI检测13项不可跳 |
| 🚨AI检测13项全查 | AI检测13项全查（**不可降级**） | **AI检测≤5%永不降级、永不裁剪、永不跳过（含纹理层）** |
| 🚨§六形容词零容忍7项+§七AI感扫描19项 | 全查（**不可降级**） | **§六/§七永不降级、永不裁剪、永不跳过；与 13 项联动、命中即修** |

裁剪触发：上下文 >20 轮。裁剪后标注"【裁剪模式】已启用"。

## 异常与 Fallback

环境层异常的预定义处理表，避免"一卡就停"。原则：**先告知用户，再按规则处理；绝不静默跳过、绝不静默 PASS**。

| 场景 | 触发条件 | 处理动作 |
|------|---------|---------|
| **Canon/状态文件缺失** | `07-memory/` 不存在或核心文件缺 | 自动触发门 0 最小骨架；写入 `pending_files` 清单，门 5 强制补建 |
| **pending_writeback=true** | 上一章未过门 5 就要写下一章 | 门 1 BLOCK；先回上一章过门 5 → pending_writeback=false → 才放行 |
| **AI 检测工具不可用** | 用户无 GPTZero / 外部 checker 不可达 | 切换 `eval_mode=self_review`：模型对 13 项逐项自评 🟢/🟡/🔴 + 必须引用具体段落作证；🔴 仍触发 BLOCK |
| **字数严重不足（<4300 且补无可补）** | 章节卡过薄、设定信息不够 | 自动回退门 1，要求用户补章节卡，或显式允许降级 Lite |
| **字数严重超出（>6000）** | 单章字数失控 | 门 4 给三选一：保留 / 拆 2 章 / 收尾压回 5000 内；不静默通过 |
| **Scene Engine 字段缺失** | scene_id 或四要素任一缺失 | 门 2 BLOCK；输出缺失字段清单；补齐前禁止进门 3 |
| **自动修复触顶** | 同门 1 次或全流程 3 次仍 BLOCK | 停止自动修复；输出当前 BLOCK 项清单 + 等用户决策 |
| **反 AI 项无证据可引用** | 模型自评 PASS 但找不到具体段落 | 视为 FAIL；要求重新检查或重写对应段落 |
| **用户要求绕过某门** | 用户说"跳过门 4"等 | 拒绝 + 引用协议 2 反绕过；提供合规替代（降 Lite / Manual Checkpoint 精简） |
| **题材或主角中途切换** | 与既有 Canon 冲突 | 触发版本分支 `Canon_v{N}_alt`，不直接覆盖；rollback_pending=true |
| **上下文超 30 轮** | 远超裁剪阈值 | 强制裁剪：仅留 SKILL.md + A 级记忆 + 最近 1 章正文，标注【强裁剪】；**一致性 C2/C5 降级 WARN**（character-engine 不在加载，详见 [consistency §6.1](./references/consistency-enforcement-protocol.md#61-裁剪模式-fallback上下文--30-轮强制裁剪时)），C1/C3/C4 仍 BLOCK 级 |
| **gate-log 写入失败** | 项目目录不可写 | 在对话内显式输出 Checklist 全文 + 提示用户手动落盘；不视为门通过 |

## 检查点协议

默认 **Auto Flow**（无暂停连贯执行）。下列节点**强制暂停**等用户确认，即便处于 Auto Flow：

| 节点 | 暂停时机 | 输出内容 |
|------|---------|---------|
| 门 0 解析完成 | 项目简报 + 主角 + 冲突 + 骨架生成后 | 1 行摘要 + "确认进门 1 / 修改 / 重做"三选一 |
| 自动修复触顶 | 同门 1 次或全流程 3 次仍 BLOCK | 列出当前 BLOCK 项 + 等用户决策 |
| 修复改动到核心设定 | 自动修复触及主角动机 / 世界规则 / Canon | 显式 diff + 等用户 OK |
| 题材或主角中途切换 | 创建 `Canon_v{N}_alt` 分支前 | 列冲突点 + 等用户决策 |
| Pro 模式关键节点 | G 大布局完成、K 群像矩阵建好、O 双轨骨架完成 | 输出对照表 + 等用户 OK |
| 用户主动触发 | 用户说"让我确认 / 暂停 / 我先看一下" | 立即从 Auto Flow 切到 Manual Checkpoint |
| 外部 AI 检测器 > 5% | GPTZero 等返回 AI 概率 > 5% 阈值 | 强制暂停 + 输出 13/7/19 项扫描报告 + 等用户选择修复路径 |

非检查点节点默认连续执行，不打断写作节奏。

## 输入要求

优先收集：平台 · 题材 · 目标读者 · 风格 · 篇幅 · 当前阶段 · 已有素材 · 约束条件。信息缺失时先补最小状态包。[详见输入收集指南](./references/input-collection-guide.md)。

## 工作流

先判断用户处于哪一阶段，加载对应模板。长篇项目最低先建：`00-brief` · `01-world` · `02-characters` · `03-outline` · `05-chapters` · `07-memory`。阶段模板列表和条件扩展树[详见 ref](./SKILL-reference.md#工作流详细规则)。

## 记忆体系

35 层 **两级正交分级**：

- **A/B/C 三级**（按"何时加载"）：A 必读(7) / B 按需(17) / C 特定(11)。详见 [memory-layer-index.md](./references/memory-layer-index.md)
- **🔥HOT / 🌡️WARM / 🧊COLD**（按"读多少"）：HOT 每章重读 / WARM 任务匹配读 / COLD 一次读 + 缓存。详见 [memory-streaming-protocol.md](./references/memory-streaming-protocol.md)

**Token 预算**：Lite ≤ 2000 / Standard ≤ 5000 / Pro ≤ 10000（A 级总）。超预算自动压缩（COLD FULL→HEAD → WARM HEAD→DELTA → HOT 缩窗）。

**记忆流（Memory Stream）**：每章门 5 回写后追加 entry 到 `07-memory/memory-stream.md`，含 6 类事实级 delta（canon / timeline / characters / foreshadow / worldmap / universemap）+ 1 类语义级 delta（deep）。续写时**优先读 stream 最近 5 章 entry，再读 canon/timeline HEAD**，token 节省 60-80%。

**🚨 第 36 层 深度记忆**（语义层）：在 35 事实层之上建 `07-memory/deep-memory.md`，含 **8 维**：主题母题 · 关系弧光 · 知识状态图谱 · 承诺账本 · 读者体验指纹 · 风格指纹 · 隐性后果 · 重复声明记录（dedup-ledger）。**Reference-once 原则** —— 设定首次说全后，后续 3 档复述权限（🔴完整 / 🟡半说 / 🟢简称），重复声明命中阈值 → 自动压缩。详见 [deep-memory-protocol.md](./references/deep-memory-protocol.md)。

**A 级 7 层（不要一次全读）**：记忆索引 · Canon · 时间线 · 人物状态 · 章节摘要 · 章节对齐单 · 十章呼应板。

**完整 35 层列表**[详见 ref](./SKILL-reference.md#记忆体系完整-35-层)。**模板映射 / 加载顺序 / 项目类型必读包**[详见 ref](./references/memory-layer-index.md)。

**版本控制**：主版本(Canon_v{N}) · 分支(Canon_v{N}_alt) · 检查点(checkpoint_{章号})。回滚后设置 `rollback_pending=true`，门1全项强制重验，不可跳过。[详见 ref](./SKILL-reference.md#版本控制机制详细)。

## 参考文档加载

**本文件**每次调用自动加载。**SKILL-reference.md** 按需加载：

| 触发 | 加载内容 |
|------|---------|
| 写正文(门3前) | 正文生成规则 + 幻觉控制 + 对话占比/群像分化/章首专项详细规则 |
| 门4不通过 | 硬检详细项 + P0/P1 修复决策树 + P1修复路由表 |
| Pro模式(G/K/O) | 增强机制 + V1-V18 Validator + 完整协议 |
| 投稿/去AI(M/N) | 润色与降AI味规则 |
| 回滚/版本分支 | 版本控制 + 回滚硬协议 |
| 其他 | 设计原则 / 审校 / 条件扩展树 / 模板索引 |

**专项 references 同步加载（不在 SKILL-reference.md 内）**：

| 触发 | 同步加载 |
|------|---------|
| 门 3 #11 反 AI 实时检查 / 门 4 P0#8 AI 检测≤5% | `references/anti-ai-thresholds.md`（13 项 + §六 形容词 + §七 AI 感扫描 SSOT）+ `references/de-ai-rewriting-patterns.md`（病灶+修法）|
| 输入信息收集 | `references/input-collection-guide.md` |
| 章首第一句去 AI | `references/first-sentence-de-ai.md` + `references/first-sentence-technique-library.md` |
| 项目目录初始化 | `references/project-organization.md` |
| 选模板 / 同名模板取舍（角色 × 6, 群像 × 3, 章节 × 6） | `references/template-index.md` |
| 选记忆层 / 35 层加载顺序 / 项目类型必读包 | `references/memory-layer-index.md` |
| 记忆流 / Hot-Warm-Cold / Token 预算 / 压缩 / Memory Stream | `references/memory-streaming-protocol.md`（项目启动 + 上下文 > 20 轮时必读）|
| 深度记忆 8 维 / 主题母题 / 关系弧光 / 知识状态 / 承诺账本 / Reference-once dedup | `references/deep-memory-protocol.md`（门 1 + 门 4 + 门 5 必读）|
| 一致性零容忍 5 类硬检（设定 / 行为 / 知识 / 时空 / 力量） | `references/consistency-enforcement-protocol.md`（门 4 P0#9 必跑）|
| 全协议闭环验证 / 6 道门 × 数据源 × BLOCK × Fallback 主矩阵 | `references/closure-map.md`（架构审计 / 调试用） |
| 输出世界地图 / 宇宙地图 / 时间线（题材特化、字段、验收、联动闭环） | `references/world-and-timeline-output-spec.md`（13 题材 × 3 维度 SSOT） |

Lite 模式 / 简单润色 / 上下文超长 → 不加载 reference，本文件已足够。
