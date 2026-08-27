---
name: skill-xiaoshuo
description: 'Use for planning, continuing, or auditing Chinese long-form fiction, web serials, and screenplay projects that need multi-chapter continuity, ensemble arcs, long foreshadowing, or layered/concurrent plots. Do not use for a standalone name, isolated paragraph, or casual idea unless the user asks to model a project.'
---

# 小说写作 Skill

> **写作执行协议系统**（Protocol-Based Writing System）。
> 规则是协议不是建议。强制执行、可验证、不可绕过。
> 先判断阶段 → 只处理当前任务 → 用户已指定项目目录且要求落盘时才写入文件；否则输出可复制的内容供用户保存。

## 核心原则

1. **目标优先级**：可读性 > 情绪价值 > 信息推进 > 人物魅力 > 结构正确性
2. **每章基准要求**：≥1 情绪兑现点 + ≥1 信息推进点（写作意识项，Checklist 自评）
3. **带证据过门**：每道门输出判定+依据，无证据的"✅通过"等同未过门
4. **反绕过**：无骨架不写正文 · 无 scene_id 不写正文 · 交稿后不回写不开下一章
5. **自动修复**：BLOCK → 自动定位+修复+重入门检查（当前门最多 1 次、全流程累计最多 3 次；触顶即请求用户决策）
6. **显式 Checklist**：所有门检查结果必须以 Checklist 形式交付；仅在用户指定项目目录并要求落盘时写入 `05-chapters/gate-logs/CH{N}-gate-log.md`，不嵌入正文
7. **自然度编辑诊断**：13 项叙事自然度、形容词和 AI 感信号用于发现解释腔、同声对白、平均句、过度工整等具体病灶；它们是**证据化的编辑诊断**，不是对 GPTZero 等检测器分数的保证。仅当病灶在正文中可定位、会伤害阅读时才回修；改写应服务人物、场景和信息，而非“骗过”检测器。[详见 ref](./references/anti-ai-thresholds.md)
8. **反公式化优先于配额**：感官、金句、对白、反转、非主角推进与“素段”等数量只作诊断信号，不是每章必须凑齐的配额。章节的视角、题材、节奏和情绪需求优先；若没有戏剧理由，宁可留白，也不硬塞。
9. **🚨 一致性硬检**：5 类硬检 —— **C1 设定** / **C2 行为** / **C3 知识** / **C4 时空** / **C5 力量**。数据源完整时，任一类 FAIL → BLOCK；当前门最多自动修复 1 次、全流程累计最多 3 次，触顶后输出 BLOCK 报告等用户决策。强裁剪导致数据源缺失时，C2/C5 只能显式标为 WARN，不能伪称已通过。[**详见 ref**](./references/consistency-enforcement-protocol.md)
10. **🚨 深度记忆 + Reference-once dedup**：35 层之外加第 36 层"深度记忆"，覆盖 **8 维语义层** —— 主题母题 / 关系弧光 / 知识状态图谱 / 承诺账本 / 读者体验指纹 / 风格指纹 / 隐性后果 / 重复声明记录。**Reference-once 原则** —— 设定 / 角色 / 物件 / 规则**首次说全后**，后续只用简称；连续 3 章重复定义即门 4 BLOCK 自动压缩。承诺账本超硬上限（埋点 + 20 章）未兑现 → 门 1 BLOCK。[**详见 ref**](./references/deep-memory-protocol.md)
11. **🚨 多层阴谋局网**：用户要求总阴谋、连环布局、幕后棋局或多重反转时，必须先建立 `03-outline/conspiracy-weave.md`：以 L0 终局局 / L1 战略局 / L2 执行局 / L3 线索载体登记因果、知识边界、资源限制、反制入口、揭示窗口与失败路径。大局可以容纳多个小局，但不能把一切都归给万能幕后人；每个重要揭示须重释旧证据并改变角色选择。[**详见协议**](./references/conspiracy-weaving-protocol.md)
12. **角色身份树与命名账本**：核心/重要角色必须有不依附主角的私人问题、独立事件线、压力选择和代价；群像以“同一命题的不同回答”交叉，而不是轮流露脸。名字、称呼与别名必须服从时代/地域/阶层、关系与知识边界；路人只需一个具体利益或反应，不强建完整传记。[**详见协议**](./references/character-identity-tree-protocol.md)
13. **并发布局与多线程伏笔**：伏笔、阴谋、候选人筛选和现实代价不是串行任务。角色可同时是不同线程的棋子、候选人、变量与布局者；一场戏可同时埋/轻触/回收多个伏笔，推进/遮蔽/反制多个局。用线程板记录表层事件、深层作用、人物位置、可见痕迹与下一次状态变化，保证并发而不混乱。[**详见协议**](./references/concurrent-plot-weave-protocol.md)

## 六道门概览

门 0 → 门 1 → 门 2 → 门 3（写正文）→ 门 4 → 门 5，串行执行，不通过不继续。

```
用户输入
   ↓
[门 0] Idea 解析 → 项目简报+主角+冲突+世界约束+骨架      （仅无结构化输入触发）
   ↓
[门 1] 写前门    → Canon/时间线/对齐已读 · 反AI预设 ·    (读 A 级 7 层 + memory-stream HEAD)
                  地图/时间线闭环 · pending_writeback=false
                  必触隐性后果检 · 承诺账本超期检 · 局网/线程压力检
   ↓
[门 2] 场景门    → scene_id + 四要素 + 反预期点          (Scene Engine SSOT)
                  + CJ-ID / 线程 ID / 真实操作 / 状态变化（如涉及局网）
   ↓
[门 3] 正文门    → 自然度诊断(13+7+19) + dedup             (按章节任务控制篇幅)
   ↓
[门 4] 质检门    → 7 项结构硬检 + C1-C5 + 39 项编辑诊断
                  P0#9 一致性 5 类 C1-C5 +
                  写作意识 #10-#15 自评 + #16/#17 双轨 + 局网公平性检
   ↓
[门 5] 回写门    → gate-log 写盘 + memory-stream 追加    (含 deep + consistency 段)
                  + deep-memory 8 维更新 + Canon/伏笔/局网/线程 delta
   ↓
pending_writeback=false → 下一章门 1
```

任一门 BLOCK → [协议 6.1 自动修复](./SKILL-reference.md#协议-61自动修复协议recovery-protocol)（同门 1 次 / 全程 3 次）→ 仍 BLOCK → 输出报告等用户决策。


| 门 | 职责 | 关键检查 | 详细 |
|----|------|---------|------|
| **门 0** | Idea 解析 | 一句话→项目简报+主角+冲突+世界约束+骨架 | 仅无结构化输入时触发 |
| **门 1** | 写前门 | Canon/时间线/对齐单已读 · 纲追溯 · 角色标签 · 回写追查 · 自然度写法预设 · 局网/活跃线程/过期揭示 · 身份树/称呼边界（触发时） | 11 项，[详见 ref](./SKILL-reference.md#门-1写前门进入正文前) |
| **门 2** | 场景门 | scene_id · 进出状态 · 类型不连续 · 非主角场景 · 反预期点 · 骨架自然度预检 · CJ-ID/线程 ID/状态变化 · 角色自主目标/场景权利（触发时） | 8 项，[详见 ref](./SKILL-reference.md#门-2场景门场景骨架完成后) |
| **门 3** | 正文门 | 映射完整 · Canon无冲突 · 篇幅符合任务 · 章首 · 对话 · 群像 · **文笔质感** · 自然度诊断 | 11 项，[详见 ref](./SKILL-reference.md#门-3正文门正文生成中后) |
| **门 4** | 质检门 | 7 项结构硬检 + C1-C5 连续性硬检 + 39 项编辑诊断 + 写作意识 + 局网公平性（触发时） | 不以互相矛盾的总数计门，[详见 ref](./SKILL-reference.md#门-4质检门正文完成后-统一质量体系) |
| **门 5** | 回写门 | 摘要/时间线/Canon/伏笔/对齐单/纲/局网更新 | 6 项，[详见 ref](./SKILL-reference.md#门-5回写门交稿后-强制闭环) |

### 门 0 最小产出

只有一句话想法/零散片段/无结构化输入时触发。必须产出：项目简报 + 主角(动机/伤口/目标) + 冲突 + 世界约束≥3条 + 总纲骨架 + 第1章章节卡 + 最小2-3场景骨架；若项目含核心群像/重要命名，再补角色身份树与命名体系。风险未转译为约束 → BLOCK。已有结构化输入 → 跳过门 0。

### 门 1 核心要求

写正文前必须确认：场景骨架+Canon+时间线+对齐单已读 · 价值声明已填 · 纲追溯通过 · 上章回写已完成(pending_writeback=false 且六项内容一致) · 本章重要人物的私人事件线、当前称呼与知识边界已读 · **自然度写法预设已确认**（本章角色声线差异点按出场需要预分配、感官通道与场景匹配）· **🚨地图/时间线闭环已建**（如项目类型需要 world-map 必建并通过 spec 验收；timeline 任何项目必建并通过 spec 验收；多界故事 universe-map 必建；详见 [world-and-timeline-output-spec.md](./references/world-and-timeline-output-spec.md)，漏建即 BLOCK）· **🚨局网触发时已读取 active CJ-ID、角色知识边界、到期揭示与反制入口**。纲体系按规模自动分类(≤3短篇/4-10中篇/11-30长篇单卷/>30多卷)。

### 门 4 Checklist 分级

门 4 的检查分为硬约束与编辑诊断两类，**执行方式不同**：

**🔒 硬检（可文件对比验证，FAIL → BLOCK）**：

P0#1目标产生可见结果（完成/失败/变形皆可）· #2冲突进展 · #3角色逻辑 · #4重大新增有铺垫或登记 · #5结尾有与章节目的相称的余波/问题/静止 · #6映射校验 · #7Canon/时间线无冲突 · **C1-C5 一致性零容忍** · #16/#17双轨一致(仅双轨)。

**自然度编辑诊断（13 项，严重且可定位时回修）**：

诊断关注四类问题：句式/段落是否过度均匀，解释是否替代了戏，角色声线与感官是否服务当前场景，信息与修辞是否妨碍人物行动。先引用具体段落，再说明它为何伤害阅读，并给出最小改法。

数字、词表和密度只是在长篇常规章中帮助发现异常的信号；独白、诗性段、极简段、回顾段和有意风格选择可以偏离。不得为了满足数字而补口语、硬造反应、塞配角、制造误解或堆“金句”。需要逐项诊断时，加载 [`references/anti-ai-thresholds.md`](./references/anti-ai-thresholds.md) 与 [`references/de-ai-rewriting-patterns.md`](./references/de-ai-rewriting-patterns.md)。

**🚨 P0#9 一致性零容忍硬检（FAIL → BLOCK，5 类全扫）**：

**C1 设定** · **C2 行为** · **C3 知识** · **C4 时空** · **C5 力量**。数据源完整时，任一类 FAIL → 当前门自动修复 1 次；全流程累计达到 3 次仍 FAIL → BLOCK 报告等用户决策。强裁剪时，C2/C5 必须显式降为 WARN，C1/C3/C4 仍为 BLOCK。详见 [consistency-enforcement-protocol.md](./references/consistency-enforcement-protocol.md)。

**📝 文笔质感（门 3 检查 + 门 4 二次验收）**：

无形容词堆砌 · 细节代替概括 · 多感官(≥2种) · 侧面描写存在 · 细节有功能(感官承载情绪/象征承载主题)

**📝 写作意识（自我评判项，输出评估+标注，不自动BLOCK）**：

#10节奏变化 · #11爽点+推进 · #12弃书风险 · #13可摘抄句（按章节需要）· #14反预期/可传播点（按章节需要）· #15标签体现 · P1全11项 · P2全5项

> 写作意识项的评估结果输出到 Checklist 中供用户审阅。P1 中 ≥3 项🔴 → Auto Flow 下自动修复最严重的 1-2 项；其余等用户确认后下一轮修复。

## 执行日志与 Checklist 输出协议

执行日志和 Checklist **不嵌入正文**。用户指定项目目录并要求落盘时，单独写入 `05-chapters/gate-logs/CH{N}-gate-log.md`；否则在对话中完整交付。正文输出保持干净。

**模板** = [`assets/gate-log-template.md`](./assets/gate-log-template.md)（7 项结构硬检 + 5 类连续性硬检 + 39 项自然度诊断 + 写作意识 + 6 项门 5 回写；R/并发布局按需另含局网与线程 delta）。

[`assets/quality-gate-template.md`](./assets/quality-gate-template.md) 仅用于投稿前、复杂章节或用户要求的深度复核；它的结论必须归并到 gate-log，不能另设 PASS / BLOCK 或修复次数。

**输出位置**：用户指定项目根并要求落盘时，写入 `05-chapters/gate-logs/CH{N}-gate-log.md`；未指定目录或未授权写入时，在对话中交付完整 Checklist，不在 cwd 自动创建文件。

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
| **Lite** | 单章试写/短篇/片段 | 门0(如需)+门1(简)+门3(核心)+门5(简) | 仅查与任务相关的结构/连续性问题；自然度做简要诊断 |
| **Standard** | 长篇续写/常规连载 | 全部6道门 | 完整 Checklist |
| **Pro** | 双轨/群像/投稿润色 | 全部6道门+完整证据 | 完整 Checklist + P2 详细 |

降级：Pro→Standard(>15轮)，Standard→Lite(>20轮)。

**任务模式映射**：P Idea / A 新建 / B 续写 / C 润色 / D 审校 / E 连载 / F 题材 / G 大布局 / H 宇宙设定 / I 写法吸收 / J 整理 / K 群像 / L 未知钩子 / M 投稿 / N 去AI / O 剧本 / Q 日更续写 / **R 多层阴谋局网**。R 为 Pro：先建局网，过节点/线索/反制/回收检查，再进入正文。默认模式和降级规则[详见 ref](./SKILL-reference.md#协议-4执行模式分级)。

## 降级与裁剪

| 正常 | 降级 | 绝对底线 |
|------|------|---------|
| 6道门全部显式 | 门0简化+门2/4可简写 | 门1/5不可跳 |
| 结构/连续性全查 + 写作意识 | 写作意识缩减 | Canon、时间线、人物知识与任务目标不可跳 |
| 自然度 39 项诊断 | 仅查当前病灶对应项 | 不静默宣称通过；有影响阅读的病灶须给证据和改法 |

裁剪触发：上下文 >20 轮。裁剪后标注"【裁剪模式】已启用"。

## 异常与 Fallback

环境层异常的预定义处理表，避免"一卡就停"。原则：**先告知用户，再按规则处理；绝不静默跳过、绝不静默 PASS**。

| 场景 | 触发条件 | 处理动作 |
|------|---------|---------|
| **Canon/状态文件缺失** | `07-memory/` 不存在或核心文件缺 | 自动触发门 0 最小骨架；写入 `pending_files` 清单，门 5 强制补建 |
| **pending_writeback=true** | 上一章未过门 5 就要写下一章 | 门 1 BLOCK；先回上一章过门 5 → pending_writeback=false → 才放行 |
| **外部 AI 检测器不可用或分数异常** | 用户未提供 checker、checker 不可达或与文本证据冲突 | 不猜测分数；输出可定位的自然度诊断。仅在用户明确要求某平台阈值且给出结果时，按其结果做定向编辑 |
| **篇幅与章节功能不匹配** | 情节过薄却拖长，或高密度章节被机械压短 | 先判断章节功能；长篇连载可默认建议 4300–5500 字，短篇/片段/剧本/润色以用户和平台要求为准，禁止灌水补字 |
| **Scene Engine 字段缺失** | scene_id 或四要素任一缺失 | 门 2 BLOCK；输出缺失字段清单；补齐前禁止进门 3 |
| **局网字段或公平性缺失** | R 模式 / 阴谋布局项目中，CJ-ID、因果、痕迹、反制或回收窗口缺失 | 门 1/2/4 BLOCK；先补 `conspiracy-weave.md`，不得用临时神秘人或事后解释替代 |
| **并发线程失控** | 多伏笔/多局/筛选项目中，线程无归属、无下次状态、人物位置不可追溯或现实代价被一次性消耗 | 门 1/2/4 BLOCK；先补 `concurrent-plot-weave.md`，合并同功能线或补后果，禁止串行化伪大局 |
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
| Manual Checkpoint 下的门 0 / R 局网完成 | 项目简报或局网骨架生成后 | 1 行摘要 + "确认进门 1 / 修改 / 重做"三选一；Auto Flow 不暂停 |
| 自动修复触顶 | 同门 1 次或全流程 3 次仍 BLOCK | 列出当前 BLOCK 项 + 等用户决策 |
| 修复改动到核心设定 | 自动修复触及主角动机 / 世界规则 / Canon | 显式 diff + 等用户 OK |
| 题材或主角中途切换 | 创建 `Canon_v{N}_alt` 分支前 | 列冲突点 + 等用户决策 |
| Pro 模式关键节点 | G 大布局完成、K 群像身份树/矩阵建好、O 双轨骨架完成、R 局网/线程板完成 | 输出对照表 + 等用户 OK |
| 用户主动触发 | 用户说"让我确认 / 暂停 / 我先看一下" | 立即从 Auto Flow 切到 Manual Checkpoint |
| 用户提供外部检测报告 | 检测结果与用户的平台要求不符 | 输出病灶与可选改法；是否继续修订由用户决定 |

非检查点节点默认连续执行，不打断写作节奏。

## 输入要求

优先收集：平台 · 题材 · 目标读者 · 风格 · 篇幅 · 当前阶段 · 已有素材 · 约束条件。信息缺失时先补最小状态包。[详见输入收集指南](./references/input-collection-guide.md)。

## 工作流

先判断用户处于哪一阶段，加载对应模板。用户指定项目目录并要求落盘时，长篇项目最低建立：`00-brief` · `01-world` · `02-characters` · `03-outline` · `05-chapters`（含 `gate-logs`）· `07-memory`（含 `memory-stream.md` 与 `deep-memory.md`）。未授权落盘时，改用“状态交接包”，不假装已建文件。阶段模板列表和条件扩展树[详见 ref](./SKILL-reference.md#工作流详细规则)。

## 记忆体系

35 层 **两级正交分级**：

- **A/B/C 三级**（按"何时加载"）：A 必读(7) / B 按需(17) / C 特定(11)。详见 [memory-layer-index.md](./references/memory-layer-index.md)
- **🔥HOT / 🌡️WARM / 🧊COLD**（按"读多少"）：HOT 每章重读 / WARM 任务匹配读 / COLD 一次读 + 缓存。详见 [memory-streaming-protocol.md](./references/memory-streaming-protocol.md)

**Token 预算**：Lite ≤ 2000 / Standard ≤ 5000 / Pro ≤ 10000（A 级总）。超预算自动压缩（COLD FULL→HEAD → WARM HEAD→DELTA → HOT 缩窗）。R 的局网是按需叠加视图，不改变“35 事实层 + 1 深度记忆”的计数。

**记忆流（Memory Stream）**：已获落盘授权时，每章门 5 回写后追加 entry 到 `07-memory/memory-stream.md`，含 6 类事实级 delta（canon / timeline / characters / foreshadow / worldmap / universemap）+ 1 类语义级 delta（deep）+ consistency；R 模式另加 `conspiracy`（局网）与 `weave`（并发线程、人物位置、现实代价）条件字段。未授权落盘时，交付同字段的“状态交接包”；下轮必须读取用户提供的交接包或项目状态文件，不能伪称已回写。续写时**优先读最近 5 章状态，再读 canon/timeline HEAD**，token 节省 60-80%。

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
| 门 3 #11 自然度诊断 / 门 4 全文编辑复查 | `references/anti-ai-thresholds.md`（13 项 + §六形容词 + §七模板感的诊断口径）+ `references/de-ai-rewriting-patterns.md`（病灶+修法）|
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
| 总阴谋 / 连环布局 / 幕后棋局 / 多重反转（R 模式） | `references/conspiracy-weaving-protocol.md` + `assets/conspiracy-weave-template.md`（门 0/1/2/4/5 局网 SSOT） |
| 角色设定 / 群像 / 人物树 / 起名 / 别名 | `references/character-identity-tree-protocol.md` + `assets/character-identity-tree-template.md`（身份树、私人事件线、命名账本 SSOT） |
| 并发伏笔 / 多局互相牵引 / 养蛊式筛选 / 底层代价 | `references/concurrent-plot-weave-protocol.md` + `assets/concurrent-plot-weave-template.md`（线程并发、遮蔽、人物位置与场景编排 SSOT） |

Lite 模式 / 简单润色 / 上下文超长 → 不加载 reference，本文件已足够。
