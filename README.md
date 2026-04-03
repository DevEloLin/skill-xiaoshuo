# skill-xiaoshuo

**写作执行协议系统**（Protocol-Based Writing System）。

这不是写作提示词集合，而是一个具备点子自动建模、强制流程控制、可验证执行、状态闭环和读者体验导向的中文小说写作操作系统。用户只需输入一个想法，系统即可自动生成完整结构并强制进入执行流程。适用于长篇小说、网文连载、设定维护、章节推进、风格统一、连续性检查和剧本双轨输出。

**三条铁律**：

| 铁律 | 含义 |
|------|------|
| **强制执行** Mandatory | 所有规则都是协议，不是建议。模型不能选择是否执行 |
| **可验证执行** Observable | 每道门必须输出判定和证据，不接受无证据的"通过" |
| **不可绕过** Non-bypassable | 用户不能要求跳过流程，门未通过则禁止继续 |

**系统目标**：目标驱动（每章必须有明确价值声明）· 自动修复（BLOCK 后自动修复，同门最多 1 次，全流程最多 3 次）· 读者导向（读者体验已并入 P0 体系）

**自主执行原则**：用户输入 → 自动结构化 → 自动执行六道门 → 自动修复错误 → 自动评估读者体验 → 自动阻断低质量输出。默认 Auto Flow（全自动），用户可随时切换 Manual Checkpoint（手动确认）

## 解决什么问题

普通写作 Prompt 常见的问题：设定漂移、人物失真、时间线混乱、伏笔遗忘、AI 擅自补设定、网文缺少追更节奏、题材同质化。

`skill-xiaoshuo` 通过执行协议系统解决这些问题：

- 先设定后正文，先提取事实再推进剧情
- 用外部状态文档维护"记忆"，写作与审校分离
- 单章字数硬约束（默认 4300-5000 字）
- 群像真正进入主线，反派和中立方也参与局势推进
- 去 AI 化变成可执行流程，章首轮换，对白分声线
- 正文不只靠视觉描写，能按需补入听觉、嗅觉、味觉、痛觉、触感和温度感
- 段落能带出隐形分镜感，用焦点、动作、物件和声响自然推进
- 伏笔多层回收，势力棋盘，未知钩子阶段揭示
- 连载前三章埋线给爽点，第 10-15 章打出首个高潮

完整设计原则见 [SKILL.md 核心设计原则](./SKILL.md#核心设计原则)。

## 系统架构

### 六道硬门

执行流程为串行门控，前一道门未通过则后续门禁止进入：

```
Idea 解析门 → 写前门 → 场景门 → 正文门 → 质检门 → 回写门
```

| 门 | 名称 | 通过证据要求 | 未通过后果 |
|----|------|-------------|-----------|
| 门 0 | Idea 解析门 | 列出生成的项目简报/主角/冲突/世界约束/骨架条目 | BLOCK：视为结构未生成 |
| 门 1 | 写前门 | 引用已读取的具体文件/事实/时间点 | BLOCK：视为未读取 |
| 门 2 | 场景门 | 列出 scene_id 列表 + 类型序列 | BLOCK：视为 Schema 未校验 |
| 门 3 | 正文门 | 引用正文中对应映射的具体段落或句子 | BLOCK：视为映射未校验 |
| 门 4 | 质检门 | P0 每项引用具体正文证据 | BLOCK：视为 P0 未执行 |
| 门 5 | 回写门 | 列出本章新增/变更的具体事实条目 | BLOCK：视为未回写 |

### 六项协议

| 编号 | 协议名称 | 作用 |
|------|---------|------|
| 协议 1 | 带证据过门 | 每道门必须附带证据，无证据的"通过"等同假执行 |
| 协议 2 | 反绕过声明（含 2.1 入口锁） | 禁止跳过、合并、推迟任何门检查 |
| 协议 3 | 降级执行 | 规则过多时的保底机制，保证核心门控不丢 |
| 协议 4 | 执行模式分级 | 按任务复杂度选择 Lite / Standard / Pro |
| 协议 5 | 执行日志 | 每轮输出开头包含可审计的执行日志头 |
| 协议 6 | 硬拒绝格式 | 门未通过时的标准拒绝输出格式 |
| 协议 6.1 | 自动修复协议 | BLOCK 后自动修复（同门最多 1 次，全流程最多 3 次），禁止改变用户意图 |

### 三级执行模式

系统根据任务复杂度自动选择模式（也可手动指定）：

| 模式 | 适用场景 | 门控范围 | 记忆层 | Validator | Quality Gate |
|------|---------|---------|--------|-----------|-------------|
| **Lite** | 单章试写、短篇、片段练习 | 门 0（如需）+ 门 1（简）+ 门 3（核心 3 项）+ 门 5（简） | Canon + 摘要 | V1+V3+V6 | P0 仅前 4 项 |
| **Standard** | 长篇续写、常规连载（默认） | 门 0（如需）+ 门 1-5 全部显式 | A 级必读 + B 级按需 | V1-V11 全查 | P0 全查 + P1 |
| **Pro** | 双轨输出、投稿润色、群像大布局 | 门 0（如需）+ 门 1-5 全部显式 + 完整证据 | A+B 级 + C 级按需 | V1-V18 全查 | P0+P1+P2 全查 |

降级规则：Pro 可降级为 Standard（上下文超 15 轮），Standard 可降级为 Lite（上下文超 20 轮）。Lite 模式下门 1 和门 5 仍不可跳过。

### Scene Engine Pipeline

场景引擎是正文和剧本的唯一真实源（Single Source of Truth），执行链路为：

```
Scene Schema (Level 1) → Mapping Layer (Level 2) → Mapping Validator (Level 3) → Quality Gate
```

- **Level 1 - Scene Schema**：23 个字段的数据契约（类型/必填/enum/校验规则），4 个 enum 合法值表，7 项章级校验
- **Level 2 - Mapping Layer**：字段级映射规则，定义 Scene 字段如何映射到小说正文和剧本格式
- **Level 3 - Mapping Validator**：V1-V11 逐场景校验 + V12-V18 双轨全章校验，共 18 项；reject/warn/pass 三级校验结果
- **Quality Gate**：P0（违反即禁止输出）/ P1（标红回修）/ P2（标注优化）三级检查

### 记忆分级

系统通过 35 层外部状态文件维护记忆，按加载优先级分为三级：

| 级别 | 层数 | 加载策略 | 包含内容 |
|------|------|---------|---------|
| **A 级** | 7 层常驻核心 | 每次长篇写作必读 | 记忆索引、Canon 事实表、时间线、人物状态、章节摘要、章节对齐、十章呼应 |
| **B 级** | 17 层条件加载 | 按当前任务加载 | 伏笔表、势力棋盘、结构网图、连续性台账、未知钩子、场景骨架、信息流、节奏、角色驱动、质量闭环、群像、冲突节奏等 |
| **C 级** | 11 层专项 | 特定模式/题材才读 | 宇宙设定、地图体系、投稿润色、去 AI 诊断、章首轮换、字数控制、情绪爆点、对白声线、情绪价值、魅力角色等 |

完整层级列表见 [SKILL.md 记忆体系](./SKILL.md#记忆体系)。

### 系统增强机制

| 机制 | 作用 | 触发时机 |
|------|------|---------|
| **全局目标函数** | 阅读体验 > 结构正确性；每章 ≥1 情绪兑现 + ≥1 信息推进 | 门 4 质检 |
| **自动修复协议** | BLOCK 后自动修复（同门最多 1 次，全流程最多 3 次），禁止改变用户意图 | 任一门 BLOCK 时 |
| **读者体验（已并入 P0）** | 爽点✘+推进✘ = P0 BLOCK，弃书风险高 = P0 BLOCK；统一在 P0/P1/P2 体系内 | 门 4 质检 |
| **本章价值声明** | 正文前必须声明核心体验/情绪价值/推进价值 | 门 1 写前 |
| **版本控制** | Canon 主版本/分支版本/检查点，禁止直接覆盖 | 门 5 回写 |
| **节奏强制反馈** | 连续 2 章无高潮 → 下章强制加入爆点/反转 | 门 1 写前检测 |
| **角色进化检测** | 连续 3 章无变化 → 角色停滞预警 | 门 4 质检 |
| **伏笔调度系统** | P1/P2/P3 分级，超期自动预警 | 门 1 写前检测 |
| **执行裁剪器** | 上下文过长时自动裁剪，保留核心门控 | 上下文 > 20 轮 |

## 核心能力

本系统通过执行协议驱动以下能力：

**基础写作**：立项、世界观、人物、总纲、分卷与章节规划、正文写作、润色与风格统一、连续性审校

**5 大控制引擎**：
- **Scene Engine（场景骨架）**：把章节拆成 2-5 个可控场景，每个场景有进入状态/冲突/退出状态；同时作为小说与剧本的唯一真实源，支持双轨输出
- **Information Flow Control（信息流控制）**：控制每章信息释放量、节奏和信息差分布
- **Pacing Engine（节奏控制）**：场景级和段落级节奏标记，防止全程一个速度
- **Character Engine（角色驱动）**：确保每章有非主角角色主动推进，防止配角工具人化
- **Quality Gate（质量闭环）**：P0/P1/P2 分级检查（读者体验已并入 P0）；前置映射校验器（Mapping Validator）逐 scene_id 校验

**长篇与连载**：35 层记忆状态体系、网文连载节奏、单章字数硬约束、章节对齐、十章回响控制、前三章埋线与阶段高潮

**结构与布局**：多线叙事、伏笔矩阵与草蛇灰线、势力棋盘、宇宙设定与世界规律、世界地图与宇宙地图

**角色与群像**：群像弧光、多阵营动态、标签化魅力角色、对白分声线、金句与有趣角色

**去 AI 化与投稿**：章首轮换、章首第一句 30 种手法库、去 AI 味诊断与精修、平台投稿适配、情绪爆点与脏感写法

**场面表现**：多感官描写、隐形分镜推进、减少纯解释场景

**题材特化**：玄幻、悬疑、言情、轻小说 + 权谋、克苏鲁、脑洞、军事、战争、谍战、搞笑、古装等扩展题材

## 适用场景

- 从零开始规划一部小说
- 已有设定，想整理成完整总纲
- 想把总纲拆成章节卡
- 想按章节卡稳定写正文
- 想做长篇或网文连载
- 想统一文风，降低 AI 味
- 想检查设定冲突、时间线冲突、人物逻辑问题
- 想按题材差异切换写作方式
- 想从小说场景骨架输出剧本格式，或同时输出小说正文和剧本（双轨输出）

## 不适用场景

- 通用营销文案
- SEO 内容生产
- 非小说类写作
- 与当前作品无关的随意灵感闲聊

## 设计原则

1. 分阶段写作，不一次性写完整部小说
2. 任何正文都必须受设定和章节目标约束
3. 长篇创作必须依赖外部状态，而不是只依赖对话上下文
4. 写作与审校分离，避免边写边自我污染
5. 信息不足时保守生成，不把猜测写成事实

## 目录结构

```text
skill-xiaoshuo/
├── README.md
├── SKILL.md
├── USAGE.md
├── work.md
├── examples/
│   ├── 01-project-init.md
│   ├── 02-continue-chapter.md
│   ├── 03-webnovel-serialization.md
│   ├── 04-genre-suspense.md
│   ├── 05-de-ai-polish.md
│   ├── 06-dialogue-voices.md
│   ├── 07-emotion-conflict-character.md
│   ├── 08-first-sentence-polish.md
│   ├── 09-length-control.md
│   ├── 10-quotable-and-funny-roles.md
│   └── README.md
├── assets/
│   ├── canon-facts-template.md
│   ├── chapter-card-template.md
│   ├── chapter-alignment-template.md
│   ├── ten-chapter-echo-template.md
│   ├── chapter-length-control-template.md
│   ├── chapter-opening-rotation-template.md
│   ├── chapter-summary-template.md
│   ├── character-biography-template.md
│   ├── character-template.md
│   ├── consistency-ledger-template.md
│   ├── continuity-checklist.md
│   ├── cosmology-template.md
│   ├── daily-continuation-template.md
│   ├── de-ai-diagnostic-template.md
│   ├── detailed-outline-template.md
│   ├── dialogue-voice-matrix-template.md
│   ├── emotion-burst-template.md
│   ├── emotional-value-board-template.md
│   ├── ensemble-arc-matrix-template.md
│   ├── ensemble-cast-template.md
│   ├── faction-board-template.md
│   ├── first-sentence-template.md
│   ├── foreshadow-matrix-template.md
│   ├── genre-lightnovel-template.md
│   ├── genre-xuanhuan-template.md
│   ├── genre-xuanyi-template.md
│   ├── genre-yanqing-template.md
│   ├── magnetic-character-template.md
│   ├── memory-index-template.md
│   ├── multi-faction-dynamics-template.md
│   ├── outline-template.md
│   ├── plot-architecture-template.md
│   ├── plot-thread-template.md
│   ├── protagonist-agency-template.md
│   ├── project-brief-template.md
│   ├── funny-role-template.md
│   ├── relationship-map-template.md
│   ├── rewrite-prompt-template.md
│   ├── serialization-board-template.md
│   ├── series-state-template.md
│   ├── storyline-board-template.md
│   ├── subplot-fragility-template.md
│   ├── submission-polish-template.md
│   ├── task-prompt-template.md
│   ├── timeline-template.md
│   ├── universe-map-template.md
│   ├── unknown-hook-matrix-template.md
│   ├── volume-outline-template.md
│   ├── world-map-template.md
│   ├── world-rules-template.md
│   ├── worldbuilding-template.md
│   ├── scene-engine-template.md
│   ├── information-flow-template.md
│   ├── pacing-engine-template.md
│   ├── character-engine-template.md
│   ├── quality-gate-template.md
│   ├── screenplay-output-template.md
│   └── conflict-rhythm-template.md
└── references/
  ├── anti-pastiche.md
  ├── chapter-length-enforcement.md
  ├── conflict-rhythm-design.md
  ├── de-ai-rewriting-patterns.md
  ├── dialogue-voice-differentiation.md
  ├── emotional-value-design.md
  ├── ensemble-writing.md
  ├── expanded-genre-strengths.md
  ├── first-sentence-de-ai.md
  ├── first-sentence-technique-library.md
  ├── hallucination-control.md
  ├── input-collection-guide.md
  ├── magnetic-character-design.md
  ├── macro-plotting.md
  ├── multisensory-scene-writing.md
  ├── popular-webnovel-craft.md
  ├── platform-submission.md
  ├── project-organization.md
  ├── quality-rubric.md
  ├── funny-role-design.md
  ├── raw-emotion-writing.md
  ├── unknown-hook-design.md
  ├── webnovel-serialization.md
  ├── world-logic.md
  ├── workflow.md
  ├── scene-engine-design.md
  ├── information-flow-control.md
  ├── pacing-engine-design.md
  ├── character-engine-design.md
  ├── quality-gate-design.md
  ├── scene-schema.md
  └── screenplay-conversion-design.md
```

## 文件说明

### 入口文件

- [SKILL.md](./SKILL.md)
  Skill 的主定义文件，负责描述何时触发、能做什么、采用什么工作流。包含六道硬门、六项协议和三级执行模式的完整定义。

- [work.md](./work.md)
  Skill 的设计草稿和迭代来源。

### 通用模板

- [project-brief-template.md](./assets/project-brief-template.md)
  用于小说立项，明确题材、受众、风格、卖点和核心冲突。

- [worldbuilding-template.md](./assets/worldbuilding-template.md)
  用于整理世界规则、势力关系、地点结构和禁忌。

- [character-template.md](./assets/character-template.md)
  用于角色动机、伤口、目标、关系网和弧光设计。

- [character-biography-template.md](./assets/character-biography-template.md)
  用于人物小传、前史、秘密、说话惯性和连续性锚点维护。

- [ensemble-cast-template.md](./assets/ensemble-cast-template.md)
  用于维护核心群像成员、反派角色、敌对阵营代表和工具人风险。

- [ensemble-arc-matrix-template.md](./assets/ensemble-arc-matrix-template.md)
  用于维护核心群像在不同阶段的弧光变化和交叉影响。

- [protagonist-agency-template.md](./assets/protagonist-agency-template.md)
  用于追踪主角每章是主动推进、被动应对还是被迫失手，防止主角长期失去主动权。

- [multi-faction-dynamics-template.md](./assets/multi-faction-dynamics-template.md)
  用于维护主角团外各阵营的目标、动作、误判和对局势的独立推进。

- [outline-template.md](./assets/outline-template.md)
  用于整部作品的大结构设计。

- [storyline-board-template.md](./assets/storyline-board-template.md)
  用于集中维护主线、支线、暗线和它们的交汇节点。

- [subplot-fragility-template.md](./assets/subplot-fragility-template.md)
  用于快速识别最容易失联、最晚必须续一口气的支线，做风险排序而不做重型扩展。

- [volume-outline-template.md](./assets/volume-outline-template.md)
  用于把整部作品拆成卷级推进，防止大纲过粗。

- [detailed-outline-template.md](./assets/detailed-outline-template.md)
  用于把卷纲和章节卡进一步细化到场景级。

- [chapter-card-template.md](./assets/chapter-card-template.md)
  用于单章情节拆解，控制本章目标、冲突、节拍和钩子。

- [scene-engine-template.md](./assets/scene-engine-template.md)
  把章节卡拆成 2-5 个可控场景。含 Scene Schema（数据契约，字段类型/必填/enum/校验规则）、字段映射规则（Mapping Layer）和映射校验器（Mapping Validator，V1-V11 逐场景校验 + V12-V18 双轨全章校验，共 18 项）。

- [information-flow-template.md](./assets/information-flow-template.md)
  用于控制每章信息释放量、释放方式和信息差分布，防止信息过密或解释集中。

- [pacing-engine-template.md](./assets/pacing-engine-template.md)
  用于标记场景级和段落级节奏，设计张力曲线，防止全章匀速推进。

- [character-engine-template.md](./assets/character-engine-template.md)
  用于分配每章角色驱动，确保非主角角色主动推进，防止配角工具人化。

- [quality-gate-template.md](./assets/quality-gate-template.md)
  用于正文写完后的 P0/P1/P2 分级检查（P0 含 scene_id、映射合规、Canon 约束，违反即禁止输出），前置映射校验器（Mapping Validator）。

- [screenplay-output-template.md](./assets/screenplay-output-template.md)
  用于从场景骨架压缩为剧本格式（INT/EXT 场景标头 + 动作描写 + 对白），支持小说与剧本双轨输出。

- [conflict-rhythm-template.md](./assets/conflict-rhythm-template.md)
  用于设计和追踪章节内冲突节奏分布、冲突类型轮换和冲突强度曲线。

- [chapter-length-control-template.md](./assets/chapter-length-control-template.md)
  用于判断章节厚度、分配字数并规定未达最低字数时先补哪里。

- [chapter-opening-rotation-template.md](./assets/chapter-opening-rotation-template.md)
  用于避免每章都从同一种方式起手，强制轮换章首类型。

- [first-sentence-template.md](./assets/first-sentence-template.md)
  用于单独控制每章第一句，避免第一句先暴露 AI 安全起手腔。

- [first-sentence-technique-library.md](./references/first-sentence-technique-library.md)
  用于提供 30 种章首第一句入口手法，只给原则和短例子，不提供死模板。

- [funny-role-template.md](./assets/funny-role-template.md)
  用于控制适量金句，以及主角团和反派队伍里的功能性有趣角色配置。

- [unknown-hook-matrix-template.md](./assets/unknown-hook-matrix-template.md)
  用于维护主未知、副未知、阶段偏移和最终回收，避免悬念只靠故弄玄虚。

### 记忆与状态模板

- [canon-facts-template.md](./assets/canon-facts-template.md)
  记录已确认且不可轻易改动的事实。

- [timeline-template.md](./assets/timeline-template.md)
  维护时间顺序，防止事件先后、伤病恢复、人物移动等逻辑冲突。

- [plot-thread-template.md](./assets/plot-thread-template.md)
  管理伏笔、线索、回收节点和信息层次。

- [chapter-summary-template.md](./assets/chapter-summary-template.md)
  记录每章发生了什么，方便下一章续写前快速回忆。

- [chapter-alignment-template.md](./assets/chapter-alignment-template.md)
  用于强制核对上一章结尾状态、本章承接点、本章兑现内容和下章不能丢的状态变化，减少前后章节不对等。

- [ten-chapter-echo-template.md](./assets/ten-chapter-echo-template.md)
  用于维护最近十章仍在持续生效的线、关系、压力、代价和总体气质，避免章节连读时像不断重新开局。

- [series-state-template.md](./assets/series-state-template.md)
  维护长篇连载当前的总状态，包括已确认事实、时间线、风险点和风格边界。

- [memory-index-template.md](./assets/memory-index-template.md)
  用于规定每次写作前的读取顺序和当前硬约束，是整个项目的记忆入口。

- [consistency-ledger-template.md](./assets/consistency-ledger-template.md)
  用于记录已锁定决定、禁止冲突项和修订记录，减少前后不对称。

- [relationship-map-template.md](./assets/relationship-map-template.md)
  用于维护人物关系温度、历史纠葛和未来变化方向。

- [cosmology-template.md](./assets/cosmology-template.md)
  用于定义宇宙层级、底层规律、世界联系和最终命题。

- [world-rules-template.md](./assets/world-rules-template.md)
  用于把世界规则写成可推导、可约束剧情的系统。

- [world-map-template.md](./assets/world-map-template.md)
  用于让地理结构真实影响资源、战争、迁徙和剧情推进。

- [universe-map-template.md](./assets/universe-map-template.md)
  用于定义世界与世界之间、文明与文明之间的宏观版图关系。

- [plot-architecture-template.md](./assets/plot-architecture-template.md)
  用于搭建主线、支线、暗线之间的结构网，避免剧情只剩单线推进。

- [foreshadow-matrix-template.md](./assets/foreshadow-matrix-template.md)
  用于把伏笔拆成近回收、中回收、远回收三层，强化草蛇灰线式埋法。

- [faction-board-template.md](./assets/faction-board-template.md)
  用于搭建势力、棋手、筹码和误判，形成更大的叙事棋盘。

### Prompt 模板

- [task-prompt-template.md](./assets/task-prompt-template.md)
  用于立项、设定、总纲、章节规划等结构化任务。

- [rewrite-prompt-template.md](./assets/rewrite-prompt-template.md)
  用于正文续写、局部改写、润色和风格统一。

- [submission-polish-template.md](./assets/submission-polish-template.md)
  用于按起点、知乎等平台偏好做定向投稿润色，重点降低 AI 痕迹和模板腔。

- [daily-continuation-template.md](./assets/daily-continuation-template.md)
  用于日更连载：用户每天输入一句话方向指令，系统自动完成状态读取、指令解析、章节卡生成、场景骨架生成和六道门执行，产出完整章节。

- [de-ai-diagnostic-template.md](./assets/de-ai-diagnostic-template.md)
  用于先诊断当前文本最重的 AI 痕迹，再决定本轮只修哪些问题，避免一轮改到失真。

### 网文连载模板

- [serialization-board-template.md](./assets/serialization-board-template.md)
  用于跟踪更新频率、爽点、悬念、卷目标、钩子类型和追更价值。

### 题材特化模板

- [genre-xuanhuan-template.md](./assets/genre-xuanhuan-template.md)
  玄幻 / 修仙 / 高武方向模板。

- [genre-xuanyi-template.md](./assets/genre-xuanyi-template.md)
  悬疑 / 推理 / 都市迷局方向模板。

- [genre-yanqing-template.md](./assets/genre-yanqing-template.md)
  言情 / 双强 / 情感成长方向模板。

- [genre-lightnovel-template.md](./assets/genre-lightnovel-template.md)
  轻小说 / 校园 / 日常冒险 / 群像方向模板。

### 参考文档

- [workflow.md](./references/workflow.md)
  定义从立项到连载维护的标准流程。

- [quality-rubric.md](./references/quality-rubric.md)
  定义结构、人设、语言、连载质量标准。

- [de-ai-rewriting-patterns.md](./references/de-ai-rewriting-patterns.md)
  总结平均句、解释腔、对白同声、现场感不足等常见 AI 痕迹的具体修法。

- [hallucination-control.md](./references/hallucination-control.md)
  规定如何减少 AI 幻觉，尤其是设定幻觉和剧情幻觉。

- [webnovel-serialization.md](./references/webnovel-serialization.md)
  规定网文连载时的开篇、单章、卷末和钩子策略。

- [macro-plotting.md](./references/macro-plotting.md)
  规定如何构建多线叙事、远近伏笔和更大的剧情棋盘。

- [world-logic.md](./references/world-logic.md)
  规定如何让宇宙、世界、地图和剧情逻辑真正对上。

- [popular-webnovel-craft.md](./references/popular-webnovel-craft.md)
  提炼热门网络小说常见长板，抽象成可复用的结构、节奏、群像、升级、悬疑、史诗感等写法机制。

- [anti-pastiche.md](./references/anti-pastiche.md)
  把"吸收长板"约束成一套硬规则，防止设定、人物、节奏、气质变成热门作品拼装。

- [ensemble-writing.md](./references/ensemble-writing.md)
  规定什么才算真正的群像、如何避免多人围观单主角，以及如何在章节和卷级控制群像平衡。

- [unknown-hook-design.md](./references/unknown-hook-design.md)
  规定什么是有效未知、什么时候投放未知钩子、如何阶段揭示，以及如何避免空洞谜语。

- [platform-submission.md](./references/platform-submission.md)
  规定投稿到起点、知乎等平台时，如何排查和降低 AI 痕迹、解释腔、平均腔和角色同声腔。

- [multisensory-scene-writing.md](./references/multisensory-scene-writing.md)
  规定如何让场面不只停留在视觉层，并通过隐形分镜式推进减少解释感。

- [expanded-genre-strengths.md](./references/expanded-genre-strengths.md)
  汇总权谋、玄幻、克苏鲁、脑洞、军事、战争、谍战、悬疑、搞笑、古装等方向的专项写法约束，并补强强代入感要求。

- [project-organization.md](./references/project-organization.md)
  给出小说项目的推荐目录树，明确故事线、总纲、细纲、人物、伏笔、记忆、审校应分别放在哪个文件夹。

- [scene-engine-design.md](./references/scene-engine-design.md)
  定义 6 种场景类型、场景组合规则、Scene 与章节/字数的关系，以及 Scene 作为统一中间层的设计。

- [information-flow-control.md](./references/information-flow-control.md)
  定义信息释放 4 条核心规则、4 种信息差类型和信息节奏模式，防止解释集中和信息过密。

- [pacing-engine-design.md](./references/pacing-engine-design.md)
  定义场景级/段落级/章级三层节奏控制、节奏与情绪的对应关系和节奏诊断方法。

- [character-engine-design.md](./references/character-engine-design.md)
  定义 4 种驱动力层级、角色工具人化 5 个信号和补救方式，解决"只有主角在动"的核心问题。

- [quality-gate-design.md](./references/quality-gate-design.md)
  定义 P0/P1/P2 三级质量检查体系（P0 禁止输出 / P1 标红回修 / P2 标注优化）、映射校验器前置门控、三层拦截架构（写前/写后/优化）和回修流程。

- [scene-schema.md](./references/scene-schema.md)
  Scene 的唯一数据契约：23 个字段的类型/必填/enum/校验规则集中定义，4 个 enum 合法值表，7 项章级校验，reject/warn/pass 三级校验结果。

- [screenplay-conversion-design.md](./references/screenplay-conversion-design.md)
  定义 Scene 作为唯一真实源的双轨输出架构、字段级映射规则（Mapping Layer）的执行逻辑、3 条转换规则（结构继承/表达转换/信息等价）、双轨一致性检查（9 项）和退化检测信号。

## 工作流

默认流程：立项 -> 设定 -> 总纲 -> 卷纲 -> 细纲 -> 章节卡 -> 场景骨架 -> 正文 -> 质检 -> 状态回写。

系统支持 17 种工作模式（A-Q）：

| 模式 | 名称 | 说明 |
|------|------|------|
| A | 新建小说 | 立项 -> 设定 -> 总纲 -> 前几章章节卡 |
| B | 继续写某一章 | 读取状态 -> 补章节卡 -> 写正文 -> 审校 -> 更新状态 |
| C | 润色现有章节 | 读取原文与风格规则 -> 局部修订 -> 保持剧情不变 |
| D | 连续性审校 | 只找问题，不负责大段重写 |
| E | 网文连载 | 读取连载状态 -> 评估追更价值 -> 生成章节卡 -> 写正文 -> 回写连载节奏 |
| F | 题材特化 | 先加载对应题材模板，再生成设定、总纲或正文 |
| G | 大布局 | 先建立结构网图、伏笔矩阵、势力棋盘，再设计卷级推进 |
| H | 宇宙设定 | 先定义宇宙运作、世界规律、地图体系，再反推文明和剧情 |
| I | 写法吸收 | 先定义主体验，再选择长板组合，最后用反拼贴规则做验收 |
| J | 项目整理 | 先输出目录树，再按阶段创建文件夹和对应状态文件 |
| K | 群像 | 先建立群像总表、弧光矩阵和关系图，再反推主线分工与章节戏份 |
| L | 未知钩子 | 先定义主未知、副未知和阶段揭示，再反推吸引力设计 |
| M | 平台投稿 | 先识别平台偏好和 AI 痕迹，再做定向去 AI 化和投稿润色 |
| N | 去 AI 味精修 | 先诊断最重的 1-3 个问题，再按节奏、解释、对白、现场感分轮修订 |
| O | 剧本输出 | 从已有场景骨架逐场景压缩为标准剧本格式；支持单独输出或与小说双轨输出 |
| P | Idea 模式 | 用户给出一句话想法，系统自动生成项目简报、主角、冲突、世界约束、三幕结构、第 1 章章节卡和场景骨架 |
| Q | 日更一句话续写 | 一句方向指令 → 自动读取状态 → 解析 → 章节卡 → 骨架 → 正文 |

完整工作流规则、源信息优先级、条件扩展树见 [SKILL.md 工作流](./SKILL.md#工作流)。

## 记忆体系

系统通过 35 层外部状态文件维护"记忆"，分为 A 级（7 层常驻核心）、B 级（17 层条件加载）、C 级（11 层专项）三级。核心包括 Canon 事实、人物状态、时间线、伏笔、章节摘要、势力棋盘、结构网图、宇宙设定、地图体系、场景骨架（含 scene_id）、信息流控制、节奏控制、角色驱动、质量闭环等。

目的：续写时快速恢复上下文，减少 AI 擅自发明新设定。每次写作前由记忆索引（A 级第 10 层）决定本轮读取哪些层。

完整层级列表见 [SKILL.md 记忆体系](./SKILL.md#记忆体系)。

## 专项能力

- **网文连载**：管理追更节奏、开篇钩子、三章承诺、十章高潮、卷末兑现。见 [SKILL.md S11](./SKILL.md#11-网文连载特化要求)
- **题材特化**：玄幻、悬疑、言情、轻小说 4 套模板 + 权谋/克苏鲁/军事等扩展题材。见 [SKILL.md S1.2](./SKILL.md#12-题材模板选择)
- **大布局**：主线/支线/暗线结构网 + 伏笔矩阵 + 势力棋盘。见 [references/macro-plotting.md](./references/macro-plotting.md)
- **宇宙与地图**：宇宙层 -> 世界层 -> 地图层 -> 剧情层四级约束。见 [references/world-logic.md](./references/world-logic.md)
- **热门写法吸收**：抽取高层写法机制，反拼贴验收。见 [references/popular-webnovel-craft.md](./references/popular-webnovel-craft.md)
- **剧本双轨输出**：Scene 作为唯一真实源，同时输出小说正文和标准剧本格式。见 [references/screenplay-conversion-design.md](./references/screenplay-conversion-design.md)

## 如何使用

### 3 步快速启动（新用户从这里开始）

不需要一次性读完全部文档。只需 3 步就能开始写作：

**第 1 步：告诉 AI 你要写什么**
> 一句话就够："一个能看到别人死亡前10分钟的女主"

系统会自动进入门 0（Idea 解析），生成项目简报、主角设计、世界约束、三幕结构、第 1 章章节卡和场景骨架，然后引导你确认后进入正文流程。

**第 2 步：跟着流程走**

AI 会按 立项 -> 设定 -> 总纲 -> 章节卡 -> 场景骨架 -> 正文 的顺序引导你。每一步都有对应模板，你不需要提前知道模板名称。

**第 3 步：写完一章后回写状态**

每章写完后，系统会自动经过质检门和回写门。这保证下一章能正确续接。

### 执行模式选择

| 模式           | 适用场景            | 复杂度            |
| ------------ | --------------- | -------------- |
| **Lite**     | 单章试写、短篇、片段练习    | 低（核心检查 + 简化门控） |
| **Standard** | 长篇续写、常规连载（默认）   | 中（6 道门全查）      |
| **Pro**      | 双轨输出、投稿润色、群像大布局 | 高（全量检查 + 完整证据） |

### 传统入门路径

1. 用 [project-brief-template.md](./assets/project-brief-template.md) 做立项
2. 用 [worldbuilding-template.md](./assets/worldbuilding-template.md) + [character-template.md](./assets/character-template.md) 做设定
3. 用 [outline-template.md](./assets/outline-template.md) 生成总纲
4. 用 [chapter-card-template.md](./assets/chapter-card-template.md) 规划章节
5. 用 [scene-engine-template.md](./assets/scene-engine-template.md) 拆场景骨架
6. 写正文 -> 质检门 -> 回写门 -> 更新状态

### 完整使用指南

详细使用指南、17 种模式的推荐提问和输入模板见 [USAGE.md](./USAGE.md)。

推荐输入信息和各场景输入包见 [references/input-collection-guide.md](./references/input-collection-guide.md)。

## 质量与反幻觉

正文输出前必须通过质检门（门 4）：P0 项违反即禁止输出，P1 项标红回修，P2 项标注优化。P0 检查包括 scene_id 合规、映射合规、Canon 约束等。

反幻觉策略：先提取事实、不擅自新增重大 canon、信息不足时保守输出。详见 [hallucination-control.md](./references/hallucination-control.md) 和 [quality-rubric.md](./references/quality-rubric.md)。

## 后续扩展方向

- 更多题材模板（都市、科幻、历史、无限流）
- 更细的男频/女频平台化策略
- 开篇三章模板、十章节奏模板、卷末爆点模板

## 相关文件

- [SKILL.md](./SKILL.md)
- [USAGE.md](./USAGE.md)
- [work.md](./work.md)
- [examples](./examples)
- [assets](./assets)
- [references](./references)

## 示例

如果你想直接照着用，可以从这些示例开始：

- [examples/README.md](./examples/README.md)
- [examples/01-project-init.md](./examples/01-project-init.md)
- [examples/02-continue-chapter.md](./examples/02-continue-chapter.md)
- [examples/03-webnovel-serialization.md](./examples/03-webnovel-serialization.md)
- [examples/04-genre-suspense.md](./examples/04-genre-suspense.md)

## 一句话总结

`skill-xiaoshuo` 是一个目标驱动、自动修复、读者导向的中文小说写作执行协议系统——六道硬门串行门控，一个点子进去，一整章稳定可连载的内容出来。
