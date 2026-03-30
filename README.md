# skill-xiaoshuo

一个面向中文小说创作的完整写作 Skill。

它的目标不是一次性“写一段像小说的文字”，而是把小说创作拆成稳定、可持续、可连载、可审校的工作流，适合用于长篇小说、网文连载、设定维护、章节推进、风格统一和连续性检查。

## 这个 Skill 解决什么问题

普通写作 Prompt 常见的问题是：

- 只会临时生成正文，不会维护设定
- 写到中后期人物开始漂移
- 世界观和时间线越来越乱
- 伏笔埋了之后忘记回收
- 为了制造戏剧性擅自新增设定
- 网文章节能写，但缺少追更节奏
- 不同题材共用同一套写法，最后变得同质化
- 主线能跑，但支线发虚，暗线缺失
- 伏笔只有单点，回收时不够成立
- 世界看起来很大，但真正推动剧情的棋盘太小

`skill-xiaoshuo` 的设计重点是：

- 先设定，后正文
- 先提取事实，再推进剧情
- 用外部状态文档维护“记忆”
- 把写作、润色、审校拆开处理
- 尽量减少 AI 幻觉和设定漂移

## 核心能力

当前版本覆盖以下能力：

1. 小说立项
2. 世界观设定
3. 人物设定
4. 总纲生成
5. 分卷与章节规划
6. 正文写作
7. 润色与风格统一
8. 连续性审校
9. 长篇记忆维护
10. 网文连载节奏控制
11. 题材特化模板切换
12. 多线叙事结构设计
13. 伏笔矩阵与草蛇灰线设计
14. 势力棋盘与宏大布局设计
15. 宇宙设定与世界规律设计
16. 世界地图与宇宙地图设计

## 适用场景

- 从零开始规划一部小说
- 已有设定，想整理成完整总纲
- 想把总纲拆成章节卡
- 想按章节卡稳定写正文
- 想做长篇或网文连载
- 想统一文风，降低 AI 味
- 想检查设定冲突、时间线冲突、人物逻辑问题
- 想按题材差异切换写作方式

## 不适用场景

- 通用营销文案
- SEO 内容生产
- 非小说类写作
- 与当前作品无关的随意灵感闲聊

## 设计原则

这个 Skill 的核心原则有 5 条：

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
├── examples/
│   ├── 01-project-init.md
│   ├── 02-continue-chapter.md
│   ├── 03-webnovel-serialization.md
│   ├── 04-genre-suspense.md
│   └── README.md
├── work.md
├── assets/
│   ├── canon-facts-template.md
│   ├── chapter-card-template.md
│   ├── chapter-summary-template.md
│   ├── character-template.md
│   ├── cosmology-template.md
│   ├── continuity-checklist.md
│   ├── faction-board-template.md
│   ├── foreshadow-matrix-template.md
│   ├── genre-lightnovel-template.md
│   ├── genre-xuanhuan-template.md
│   ├── genre-xuanyi-template.md
│   ├── genre-yanqing-template.md
│   ├── outline-template.md
│   ├── plot-architecture-template.md
│   ├── plot-thread-template.md
│   ├── project-brief-template.md
│   ├── rewrite-prompt-template.md
│   ├── serialization-board-template.md
│   ├── series-state-template.md
│   ├── task-prompt-template.md
│   ├── timeline-template.md
│   ├── universe-map-template.md
│   ├── world-map-template.md
│   ├── world-rules-template.md
│   └── worldbuilding-template.md
└── references/
  ├── macro-plotting.md
    ├── hallucination-control.md
    ├── quality-rubric.md
    ├── webnovel-serialization.md
  ├── world-logic.md
    └── workflow.md
```

## 文件说明

### 入口文件

- [SKILL.md](./SKILL.md)
  这是 Skill 的主定义文件，负责描述何时触发、能做什么、采用什么工作流。

- [work.md](./work.md)
  这是这个 Skill 的设计草稿和迭代来源。

### 通用模板

- [project-brief-template.md](./assets/project-brief-template.md)
  用于小说立项，明确题材、受众、风格、卖点和核心冲突。

- [worldbuilding-template.md](./assets/worldbuilding-template.md)
  用于整理世界规则、势力关系、地点结构和禁忌。

- [character-template.md](./assets/character-template.md)
  用于角色动机、伤口、目标、关系网和弧光设计。

- [outline-template.md](./assets/outline-template.md)
  用于整部作品的大结构设计。

- [chapter-card-template.md](./assets/chapter-card-template.md)
  用于单章情节拆解，控制本章目标、冲突、节拍和钩子。

### 记忆与状态模板

- [canon-facts-template.md](./assets/canon-facts-template.md)
  记录已确认且不可轻易改动的事实。

- [timeline-template.md](./assets/timeline-template.md)
  维护时间顺序，防止事件先后、伤病恢复、人物移动等逻辑冲突。

- [plot-thread-template.md](./assets/plot-thread-template.md)
  管理伏笔、线索、回收节点和信息层次。

- [chapter-summary-template.md](./assets/chapter-summary-template.md)
  记录每章发生了什么，方便下一章续写前快速回忆。

- [series-state-template.md](./assets/series-state-template.md)
  维护长篇连载当前的总状态，包括已确认事实、时间线、风险点和风格边界。

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

- [hallucination-control.md](./references/hallucination-control.md)
  规定如何减少 AI 幻觉，尤其是设定幻觉和剧情幻觉。

- [webnovel-serialization.md](./references/webnovel-serialization.md)
  规定网文连载时的开篇、单章、卷末和钩子策略。

- [macro-plotting.md](./references/macro-plotting.md)
  规定如何构建多线叙事、远近伏笔和更大的剧情棋盘。

- [world-logic.md](./references/world-logic.md)
  规定如何让宇宙、世界、地图和剧情逻辑真正对上。

## 工作流

### 标准流程

默认采用以下顺序：

1. 立项
2. 设定
3. 总纲
4. 章节卡
5. 正文
6. 审校
7. 状态回写

这套顺序的意义是避免正文先跑太远，后面再回头硬补设定。

### 支持的工作模式

Skill 当前支持 6 种主模式：

1. 模式 A，新建小说
2. 模式 B，继续写某一章
3. 模式 C，润色现有章节
4. 模式 D，连续性审校
5. 模式 E，网文连载模式
6. 模式 F，题材特化模式

### 源信息优先级

当不同输入发生冲突时，按以下优先级裁决：

1. 用户明确给出的硬约束
2. Canon 事实表
3. 世界观与角色设定
4. 最近章节摘要和时间线
5. 总纲和章节卡
6. 推测性补全

这条规则用于降低长篇创作时的逻辑漂移。

## 记忆体系

这个 Skill 不把“记忆”交给模型临时发挥，而是显式维护 10 类状态：

1. Canon 事实
2. 人物状态
3. 时间线
4. 伏笔与线索
5. 章节摘要
6. 连载节奏
7. 主线 / 支线 / 暗线结构网
8. 势力棋盘
9. 宇宙设定
10. 地图体系

这样做的目的有两个：

- 续写时能快速恢复上下文
- 减少 AI 擅自发明新设定的空间

### 为什么这样能减少幻觉

因为很多所谓“AI 幻觉”，本质上是：

- 输入不完整
- 事实边界不明确
- 没有区分“已知事实”和“待确认内容”

这个 Skill 通过外部状态模板把这些边界显式化，从而减少：

- 设定凭空新增
- 角色行为失真
- 时间线冲突
- 伏笔遗忘
- 文风突然变调

## 网文连载特化

这个 Skill 已经内置网文连载模式，不只是能“写章节”，而是能管理追更节奏。

### 连载模式重点控制什么

- 开篇钩子是否足够快
- 三章内是否建立核心承诺
- 十章内是否形成追更理由
- 每章是否有推进，而不是空转
- 卷末是否有兑现、升级或反转

### 连载模式适合什么类型

- 男频升级流
- 女频情感成长线
- 悬疑连载
- 轻小说群像连载
- 长篇网文项目

## 题材模板系统

为了避免不同题材使用同一套写法，Skill 当前支持 4 套题材模板：

- 玄幻
- 悬疑
- 言情
- 轻小说

这些模板不是简单分类，而是分别定义：

- 该题材读者真正期待什么
- 设定要重点控制什么
- 结构上应如何推进
- 常见错误是什么

这能显著减少“题材表面像，内核不对”的问题。

## 大布局与草蛇灰线

这次优化后，这个 Skill 不再只强调“稳”，也强调“大”。

这里说的“大”，不是单纯把世界观写得很大，而是：

- 至少有主线、支线、暗线三层结构
- 伏笔不是一个点，而是多点、多层、多时距
- 势力和人物的博弈能形成棋盘
- 单章推进会反作用于更大的局势

对应文件：

- [assets/plot-architecture-template.md](./assets/plot-architecture-template.md)
- [assets/foreshadow-matrix-template.md](./assets/foreshadow-matrix-template.md)
- [assets/faction-board-template.md](./assets/faction-board-template.md)
- [references/macro-plotting.md](./references/macro-plotting.md)

## 宇宙、世界与地图系统

这次优化后，这个 Skill 也不再把世界观当成单独设定页，而是把它拆成四个彼此约束的层级：

- 宇宙层：宇宙如何运作、边界在哪里、什么规律不可违背
- 世界层：文明、资源、力量、秩序如何运行
- 地图层：地区和世界之间如何连通、阻隔、冲突和迁移
- 剧情层：这些规律如何实际限制人物、势力和事件

对应文件：

- [assets/cosmology-template.md](./assets/cosmology-template.md)
- [assets/world-rules-template.md](./assets/world-rules-template.md)
- [assets/world-map-template.md](./assets/world-map-template.md)
- [assets/universe-map-template.md](./assets/universe-map-template.md)
- [references/world-logic.md](./references/world-logic.md)

## 如何使用

### 用法 1：从零开始写一部小说

建议顺序：

1. 用 [project-brief-template.md](./assets/project-brief-template.md) 做立项
2. 用 [worldbuilding-template.md](./assets/worldbuilding-template.md) 写世界观
3. 用 [character-template.md](./assets/character-template.md) 写人物
4. 用 [outline-template.md](./assets/outline-template.md) 生成总纲
5. 用 [chapter-card-template.md](./assets/chapter-card-template.md) 规划前几章
6. 再进入正文写作

### 用法 2：继续写某一章

建议先准备：

1. [canon-facts-template.md](./assets/canon-facts-template.md)
2. [chapter-summary-template.md](./assets/chapter-summary-template.md)
3. [timeline-template.md](./assets/timeline-template.md)
4. [series-state-template.md](./assets/series-state-template.md)
5. [rewrite-prompt-template.md](./assets/rewrite-prompt-template.md)

这样续写时不容易跑偏。

### 用法 3：做网文连载

建议额外维护：

1. [serialization-board-template.md](./assets/serialization-board-template.md)
2. [series-state-template.md](./assets/series-state-template.md)
3. [plot-thread-template.md](./assets/plot-thread-template.md)

这样不仅能写，还能控制更新节奏和追更价值。

### 用法 4：做题材特化创作

写作前先选择：

- 玄幻：用 [genre-xuanhuan-template.md](./assets/genre-xuanhuan-template.md)
- 悬疑：用 [genre-xuanyi-template.md](./assets/genre-xuanyi-template.md)
- 言情：用 [genre-yanqing-template.md](./assets/genre-yanqing-template.md)
- 轻小说：用 [genre-lightnovel-template.md](./assets/genre-lightnovel-template.md)

## 推荐输入信息

为了让这个 Skill 输出更稳定，建议输入时尽量提供：

- 题材与子类型
- 目标读者
- 风格目标
- 篇幅目标
- 当前阶段
- 已有素材
- 视角要求
- 平台偏好
- 已确认 canon
- 最近章节摘要
- 当前时间线位置
- 当前人物状态
- 已埋伏笔与待回收线索

## 输出风格

这个 Skill 默认强调：

- 结构化输出
- 方便保存到 Markdown / Obsidian
- 先列已知事实，再给正文或方案
- 明确标注风险和待确认点

对长篇项目来说，这比只追求“文字好看”更重要。

## 反幻觉策略

Skill 当前采用以下策略降低幻觉：

1. 写作前先提取已知事实和未知事实
2. 不允许擅自新增重大 canon
3. 信息不足时优先给出保守版
4. 先审校再回写状态
5. 用外部模板约束长期项目

更详细规则见 [hallucination-control.md](./references/hallucination-control.md)。

## 质量门槛

一章正文至少要满足：

1. 本章目标明确
2. 本章冲突成立
3. 角色行为符合设定
4. 没有偷塞重大新设定
5. 结尾留下余波、问题或钩子

如果做不到，就应先退回章节卡或问题清单层级，而不是硬写。

## 当前版本特点

相较于一个普通小说 Prompt，这个 Skill 更像一个“小说生产工作台”，因为它已经具备：

- 主 Skill 定义
- 工作流拆分
- 结构化模板
- 状态管理模板
- 连载节奏模板
- 题材模板
- 质量标准
- 防幻觉规则

## 后续可扩展方向

如果后面继续扩展，这个 Skill 还适合加入：

- 都市模板
- 科幻模板
- 历史模板
- 无限流模板
- 开篇三章模板
- 十章节奏模板
- 卷末爆点模板
- 更细的男频 / 女频平台化策略

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

`skill-xiaoshuo` 是一个围绕“稳定写长篇、会维护状态、能控制连载节奏、能减少 AI 幻觉”设计的中文小说写作 Skill。