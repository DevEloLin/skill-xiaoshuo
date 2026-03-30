# skill-xiaoshuo 使用文档

这是一份面向实际操作的使用文档。

它不再解释这个 Skill 是什么，而是直接回答下面几个问题：

1. 我应该怎么开始使用它
2. 每一阶段应该怎么提问
3. 长篇和连载为什么必须维护状态文件
4. 在支持 Skill 的环境里怎么用
5. 在 Claude、Codex、Cursor 这类不完全共享同一规范的客户端里怎么迁移使用

## 一、先理解它到底怎么用

`skill-xiaoshuo` 不是一个“万能小说 Prompt”。

它更像一个小说写作工作台，正确用法不是一句话让它把整本书写完，而是：

1. 先明确当前阶段
2. 只做当前阶段的任务
3. 把关键结果保存到状态文件
4. 下一轮继续读取这些状态文件再往下写

如果你把它当成一次性生成器来用，后面仍然会出现：

- 设定漂移
- 时间线冲突
- 人物行为失真
- 伏笔遗忘
- AI 擅自新增重大设定

如果你希望剧情更大、更复杂、更有草蛇灰线感，那么还要多做三件事：

1. 先搭主线、支线、暗线结构网
2. 先铺伏笔矩阵，再写中后期推进
3. 先建立势力棋盘，再写大局碰撞

如果你希望世界观也真正撑得住宏大叙事，还要再加四件事：

4. 先定义宇宙如何运作
5. 先定义世界规则和代价
6. 先建立世界地图
7. 先建立宇宙地图

## 二、推荐使用顺序

### 1. 新建项目时

建议顺序：

1. 项目简报
2. 世界观
3. 角色设定
4. 总纲
5. 前几章章节卡
6. 正文

对应文件：

- [assets/project-brief-template.md](./assets/project-brief-template.md)
- [assets/worldbuilding-template.md](./assets/worldbuilding-template.md)
- [assets/character-template.md](./assets/character-template.md)
- [assets/outline-template.md](./assets/outline-template.md)
- [assets/chapter-card-template.md](./assets/chapter-card-template.md)

### 2. 长篇 / 连载项目时

除了上面的结构文件，必须尽早建立状态文件：

- [assets/canon-facts-template.md](./assets/canon-facts-template.md)
- [assets/timeline-template.md](./assets/timeline-template.md)
- [assets/plot-thread-template.md](./assets/plot-thread-template.md)
- [assets/chapter-summary-template.md](./assets/chapter-summary-template.md)
- [assets/series-state-template.md](./assets/series-state-template.md)

如果是网文连载，再加：

- [assets/serialization-board-template.md](./assets/serialization-board-template.md)

如果你要追求大布局，再加：

- [assets/plot-architecture-template.md](./assets/plot-architecture-template.md)
- [assets/foreshadow-matrix-template.md](./assets/foreshadow-matrix-template.md)
- [assets/faction-board-template.md](./assets/faction-board-template.md)

如果你还要追求宏伟世界观，再加：

- [assets/cosmology-template.md](./assets/cosmology-template.md)
- [assets/world-rules-template.md](./assets/world-rules-template.md)
- [assets/world-map-template.md](./assets/world-map-template.md)
- [assets/universe-map-template.md](./assets/universe-map-template.md)

### 3. 日常章节写作时

建议固定流程：

1. 读上一章摘要
2. 读 canon 事实和时间线
3. 先做章节卡
4. 再写正文
5. 写完后做审校
6. 回写章节摘要、时间线、伏笔和状态

这是这个 Skill 最关键的用法。

### 4. 大布局项目时

如果你写的是中长篇、多卷、群像、势力博弈型作品，建议在章节写作前先做三件事：

1. 结构网图：明确主线、支线、暗线怎么交汇
2. 伏笔矩阵：明确近回收、中回收、远回收
3. 势力棋盘：明确谁在布局、谁在误判、谁会翻桌

如果这三步不做，很容易出现“设定很大，但剧情还是单线跑”的问题。

### 5. 宏伟世界观项目时

如果你写的是高武、仙侠、科幻、神话、多界战争、星海文明、诸天流这类作品，建议在总纲前先做四件事：

1. 宇宙设定：定义宇宙层级和底层规律
2. 世界规律：定义时间、空间、能量、文明和因果规则
3. 世界地图：定义地理骨架、资源区、战略通道
4. 宇宙地图：定义世界之间、文明之间和通路之间的关系

如果这四步不做，后面很容易出现：

- 世界很大但剧情感知不到
- 跨界、穿梭、战争和资源逻辑对不上
- 规则只在需要时出现，不需要时消失

## 三、最推荐的 6 种使用模式

### 模式 A：新建小说

适合场景：

- 只有一个想法，还没有成型
- 想把零散灵感收敛成可写项目

推荐目标：

- 输出项目简报
- 输出世界观和人物基础卡
- 输出总纲
- 输出前三章章节卡

推荐提问：

```text
任务阶段：立项 + 设定 + 总纲
题材与子类型：都市悬疑
目标读者：青年向
风格目标：冷静、压抑、克制
篇幅目标：中长篇
已有素材：一名能看到他人死亡前最后十分钟的女主
必须保留：能力有代价
必须避免：开局直接讲清所有真相

本次任务：
1. 做项目简报
2. 补世界观基础版
3. 设计主角和主要对手
4. 输出三幕式总纲
```

### 模式 B：继续写某一章

适合场景：

- 已经有前文
- 已经有设定和状态资料
- 想稳定推进下一章

推荐目标：

- 输出本章功能理解
- 输出正文
- 输出状态回写建议

推荐提问：

```text
任务阶段：正文
题材模板：玄幻
目标读者：男频
当前章节：第18章
已确认 canon：
- 主角不能公开暴露第二灵根
- 七长老已经怀疑他
最近章节摘要：
- 主角发现试炼谷旧阵眼
- 宗门开始搜查他住处
当前时间线位置：外门大比第3天夜里
当前人物状态：主角重伤未愈，但必须应战
本章目标：确认七长老今晚动手，并推进临时联盟
必须避免：新增力量体系、突改角色身份
```

### 模式 C：润色现有章节

适合场景：

- 正文已有
- 剧情不想大改
- 想降 AI 味、统一人物口吻、加强画面感

推荐提问：

```text
任务阶段：润色
题材模板：言情
风格目标：细腻、克制、有拉扯感
本次处理重点：压缩解释、增强人物区分、降低套话
不可改变项：剧情结果 / 世界规则 / 关键伏笔

原文：
<粘贴原文>
```

### 模式 D：连续性审校

适合场景：

- 已写了多个章节
- 担心设定、时间线、人物逻辑出了问题

推荐目标：

- 只找问题
- 不直接重写

推荐提问：

```text
任务阶段：审校
审校范围：第12章到第18章
重点检查：时间线 / 人物动机 / 伏笔 / 规则一致性
已确认 canon：
<粘贴关键事实>
待检查正文：
<粘贴正文或摘要>
```

### 模式 E：网文连载模式

适合场景：

- 日更或周更
- 需要章尾钩子
- 需要控制追更节奏

推荐同时提供：

- 当前卷目标
- 最近三章推进点
- 当前看点
- 当前悬念
- 本章钩子类型

推荐提问：

```text
任务阶段：网文连载模式
题材模板：言情
目标读者：女频
平台风格：女频成长向
更新频率：日更
单章字数范围：2800
当前卷目标：女主从被动执行者转向主动争夺主导权
最近三章推进点：
1. 女主发现项目数据被删改
2. 男主替她挡下高层追责
3. 女主发现男主与董事会私下会面
本章目标：推进职场线和情感线，并留下具体钩子
必须避免：整章空转、只靠误会推进
```

### 模式 F：题材特化模式

适合场景：

- 你明确知道自己写的是哪一类小说
- 你不希望 Skill 用“通用写法”处理

当前支持：

- 玄幻：[assets/genre-xuanhuan-template.md](./assets/genre-xuanhuan-template.md)
- 悬疑：[assets/genre-xuanyi-template.md](./assets/genre-xuanyi-template.md)
- 言情：[assets/genre-yanqing-template.md](./assets/genre-yanqing-template.md)
- 轻小说：[assets/genre-lightnovel-template.md](./assets/genre-lightnovel-template.md)

推荐提问：

```text
任务阶段：题材特化模式
题材与子类型：都市悬疑 / 社会派推理
题材模板：悬疑
目标读者：青年向
风格目标：冷静、写实、压抑
已有素材：
- 一名社区调解员在处理失踪案时，发现三起旧案共享同一个见证人
必须保留：谜底必须回扣前文线索
必须避免：超自然反转、最后一章空降真相

本次任务：
1. 定义核心谜题
2. 设计信息层级
3. 输出四阶段推进总纲
4. 列出可回收线索
```

### 模式 G：大布局模式

适合场景：

- 多卷长篇
- 群像叙事
- 势力博弈
- 需要草蛇灰线、千里回响的伏笔结构

推荐目标：

- 输出主线 / 支线 / 暗线结构网
- 输出伏笔矩阵
- 输出势力棋盘
- 指定每一阶段负责埋什么、偏移什么、回收什么

推荐提问：

```text
任务阶段：大布局设计
题材模板：玄幻
目标读者：男频
篇幅目标：多卷长篇
终局方向：主角从宗门弃子走到重塑天下秩序
必须保留：
- 有宗门、王朝、上古遗留三层棋盘
- 主角个人成长要与世界格局变化绑定
必须避免：
- 只靠升级推剧情
- 支线和主线彼此无关

本次任务：
1. 搭一个主线、三条支线、一条暗线的结构网
2. 设计近中远三层伏笔
3. 给出三方势力棋盘
4. 标出哪些节点适合做草蛇灰线式回收
```

### 模式 H：宇宙设定模式

适合场景：

- 高武 / 仙侠 / 神话 / 科幻 / 多界叙事
- 需要建立宇宙规则、边界和地图体系
- 希望世界观能真正约束剧情逻辑

推荐目标：

- 输出宇宙设定
- 输出世界规律
- 输出世界地图
- 输出宇宙地图
- 标出这些规则如何进入剧情

推荐提问：

```text
任务阶段：宇宙设定
题材模板：玄幻
篇幅目标：多卷长篇
宇宙层级：多界 + 上界 + 禁域
希望效果：世界观宏伟，但规则必须自洽，地图和剧情要能对应
必须保留：
- 不同世界之间有明确通路和代价
- 力量体系和宇宙规则挂钩
- 势力扩张必须受资源和地理限制
必须避免：
- 地图只是地名列表
- 宇宙规则只在需要时出现

本次任务：
1. 设计宇宙运作机制
2. 设计世界规律
3. 设计世界地图和宇宙地图
4. 说明这些规则如何约束主线和势力棋盘
```

## 四、每次输入时最好提供什么

### 最小输入包

如果你只想先开始，至少给这些：

- 题材与子类型
- 目标读者
- 风格目标
- 篇幅目标
- 当前阶段
- 已有素材
- 必须保留
- 必须避免

### 长篇输入包

如果你要写长篇或续写，最好再加：

- 已确认 canon
- 最近章节摘要
- 当前时间线位置
- 当前人物状态
- 已埋伏笔与待回收线索
- 不能更改的设定

### 大布局输入包

如果你想让故事不单一，最好再补：

- 主线目标与终局方向
- 至少 2 到 4 条支线
- 至少 1 条暗线
- 关键势力或棋手
- 近回收 / 中回收 / 远回收伏笔层级
- 哪些章节负责埋，哪些章节负责转向，哪些章节负责回收

### 宏伟世界观输入包

如果你想让世界观和剧情逻辑对上，最好再补：

- 宇宙层级
- 世界运作核心
- 绝对不可违背的规律
- 可以被发现或改写的规律
- 世界地图骨架
- 宇宙地图骨架
- 资源流动、交通路径、战争路径、迁徙路径
- 哪条规律会在后期变成关键伏笔或终局解法

### 连载输入包

如果你在写网文，再加：

- 更新频率
- 单章字数范围
- 平台风格
- 当前卷目标
- 最近三章推进点
- 当前看点
- 当前悬念
- 本章钩子目标

## 五、输出结果应该长什么样

这个 Skill 最推荐的输出结构是：

1. 当前任务目标
2. 关键输入假设
3. 结构化产出
4. 风险或待确认点
5. 下一步建议

如果任务是正文，可以把正文放在第 3 项，但仍然建议在正文前先说明：

- 本章目标
- 本章冲突
- 本章情绪走向

## 六、为什么一定要维护状态文件

长篇写作真正难的不是写某一段，而是连续 20 章、50 章、100 章还不崩。

这个 Skill 之所以强调状态文件，是因为模型本身不会自动可靠地长期记住：

- 角色之前说过什么
- 哪个伏笔埋在哪里
- 伤势恢复需要多久
- 哪条规则不能被推翻

所以推荐你把这些信息外置成 Markdown 文件。

最小状态集合建议是：

1. Canon 事实表
2. 时间线
3. 伏笔与线索表
4. 章节摘要

如果你要写大格局，再加：

5. 结构网图
6. 伏笔矩阵
7. 势力棋盘

如果你还要写宏伟世界观，再加：

8. 宇宙设定书
9. 世界规律表
10. 世界地图
11. 宇宙地图

如果是连载，再加：

12. 连载状态板
13. 连载节奏板

## 七、最常见的错误用法

### 错误 1：一上来就写整本书

这会导致设定没锁住，后面大概率崩掉。

### 错误 2：没有章节卡就直接写正文

这会导致单章功能不清，容易空转。

### 错误 3：写完不回写状态

这会导致下一章开始时信息断层。

### 错误 4：不给约束，只说“帮我续写”

这类输入过于模糊，模型会保守输出，或者开始自由补设定。

### 错误 5：让它一边写一边审校

写作和审校最好分开，否则容易边写边污染判断。

## 八、在不同客户端里怎么用

这里要先区分一件事：

`skill-xiaoshuo` 当前是按 VS Code / Copilot 风格的 Skill 组织的。不同客户端不一定原生识别同一套格式。

所以正确理解不是“把同一个文件夹丢给所有客户端都自动生效”，而是：

- 这个目录是你的“能力源文件”
- 在不同客户端里，用各自支持的自定义机制承接它

### 1. 在 VS Code / Copilot 里

这是最接近原生 Skill 用法的环境。

推荐方式：

1. 保持当前目录结构不变
2. 通过 `description` 中的触发词自然调用
3. 或者直接手动调用 `/skill-xiaoshuo`
4. 把产物保存到当前工作区的 Markdown 文件中

适合的调用方式：

- `/skill-xiaoshuo 帮我做一部悬疑小说的立项和总纲`
- `继续写第12章，先读取我的 canon 和时间线`
- `帮我审校第20章到第25章有没有时间线冲突`

### 2. 在 Claude 里

Claude 不一定直接识别 Copilot 的 `SKILL.md` 规范，但可以很好地承接同样的能力结构。

推荐做法：

1. 保留当前目录作为知识源
2. 如果你的 Claude 工作流支持 `.claude/skills/`，可以把这个 Skill 镜像过去
3. 如果不支持 Skill 目录，就把核心规则压缩成一份 Claude 专用说明文件
4. 在对话中明确要求 Claude 先读取这些文件再执行

最实用的迁移方法：

- 保留 [SKILL.md](./SKILL.md) 作为总说明
- 保留 [assets](./assets) 作为模板库
- 保留 [references](./references) 作为规则库
- 给 Claude 一个入口提示，例如：

```text
你现在按本项目的小说写作工作流工作。
先遵循 SKILL.md 的阶段化写作规则。
如果是长篇任务，先读取 canon、时间线、伏笔和章节摘要再继续。
如果是网文任务，优先读取连载节奏板。
如果是玄幻/悬疑/言情/轻小说，优先按对应题材模板执行。
```

推荐目录适配方式：

```text
.claude/skills/skill-xiaoshuo/
  SKILL.md
  assets/
  references/
```

如果你的 Claude 环境不支持上面这种目录结构，就至少保留一份入口说明和模板目录。

### 3. 在 Cursor 里

Cursor 更适合通过项目规则和上下文文件来承接这个 Skill，而不是直接期待它识别 Copilot 的 Skill 规范。

推荐做法：

1. 把当前 `skill-xiaoshuo` 目录保留在项目内
2. 在 Cursor 的项目规则中写一条小说写作规则
3. 规则中要求模型优先读取：
   - `SKILL.md`
   - `assets/`
   - `references/`
4. 如果是长篇任务，要求先读取状态文件再写正文

Cursor 里的核心不是“复制 Skill 机制”，而是复制这套工作流：

- 阶段化
- 状态化
- 模板化
- 审校分离

可以给 Cursor 一条项目规则，内容类似：

```text
当任务涉及小说创作、续写、章节规划、人物设定、总纲、网文连载、审校时：
1. 先读取 skill-xiaoshuo/SKILL.md
2. 根据任务阶段选择对应模板
3. 长篇任务优先读取 canon、时间线、伏笔和章节摘要
4. 信息不足时先列待确认项，不要擅自新增重大设定
5. 写作和审校分开执行
```

### 4. 在 Codex 里

Codex 类工作流通常更适合通过仓库级说明文件来承接这一套规则，而不是直接期待它识别 `SKILL.md`。

推荐做法：

1. 把 `skill-xiaoshuo` 保留为仓库中的能力目录
2. 增加一个仓库级入口说明，例如 `AGENTS.md` 或同类说明文件
3. 在入口说明里要求代理在处理小说相关任务时优先读取：
   - `skill-xiaoshuo/SKILL.md`
   - `skill-xiaoshuo/assets/`
   - `skill-xiaoshuo/references/`
4. 把状态文件和章节产物也放在仓库中，让它可持续读取

在 Codex 里，最关键的不是目录名字，而是入口说明是否足够明确。

推荐入口说明写法：

```text
When the task is about fiction writing, novel planning, chapter drafting, serialization, revision, or continuity review:
1. Read skill-xiaoshuo/SKILL.md first
2. Use the matching template from skill-xiaoshuo/assets/
3. For long-form projects, read canon facts, timeline, plot threads, and chapter summaries before writing
4. Do not invent major canon when information is missing
5. Separate drafting from review
```

## 九、我最推荐的实际落地方式

如果你真正要长期写小说，我建议你这样组织：

```text
novel-project/
├── skill-xiaoshuo/
│   ├── SKILL.md
│   ├── USAGE.md
│   ├── assets/
│   ├── references/
│   └── examples/
├── project-brief.md
├── world.md
├── characters.md
├── outline.md
├── canon.md
├── timeline.md
├── plot-threads.md
├── series-state.md
├── serialization-board.md
└── chapters/
    ├── 001.md
    ├── 002.md
    └── ...
```

这样不管你换到 Copilot、Claude、Cursor 还是 Codex，本质上都还是同一套工作流。

## 十、推荐先做哪几步

如果你现在马上要开始用，我建议只做这 5 步：

1. 先读 [README.md](./README.md)
2. 用 [assets/project-brief-template.md](./assets/project-brief-template.md) 做立项
3. 建 4 个状态文件：canon、timeline、plot-threads、chapter-summary
4. 每章先做章节卡再写正文
5. 每章写完做审校并回写状态

## 十一、相关文件

- [README.md](./README.md)
- [SKILL.md](./SKILL.md)
- [examples/README.md](./examples/README.md)
- [assets](./assets)
- [references](./references)

## 十二、一句话使用原则

把它当成“分阶段写作系统”来用，而不是“整本小说一键生成器”。