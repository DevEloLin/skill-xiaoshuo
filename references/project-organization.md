# 小说项目文件夹组织参考

目标：

把长篇小说、连载项目、宏大世界观项目的资料收纳成固定目录，减少信息散落、记忆缺失和前后矛盾。

## 一、推荐目录树

```text
novel-project/
├── 00-brief/
│   ├── project-brief.md
│   └── promise-and-boundary.md
├── 01-world/
│   ├── worldbuilding.md
│   ├── cosmology.md
│   ├── world-rules.md
│   ├── world-map.md
│   └── universe-map.md
├── 02-characters/
│   ├── character-cards.md
│   ├── character-biographies.md
│   ├── relationship-map.md
│   ├── ensemble-cast.md
│   ├── ensemble-arc-matrix.md
│   └── character-identity-tree.md
├── 03-outline/
│   ├── master-outline.md
│   ├── storyline-board.md
│   ├── plot-architecture.md
│   ├── foreshadow-matrix.md
│   ├── faction-board.md
│   ├── conspiracy-weave.md
│   └── concurrent-plot-weave.md
├── 04-volumes/
│   ├── volume-01-outline.md
│   ├── volume-02-outline.md
│   └── ...
├── 05-chapters/
│   ├── chapter-cards/
│   ├── detailed-outlines/
│   ├── chapter-summaries/
│   └── gate-logs/
├── 06-drafts/
│   ├── volume-01/
│   ├── volume-02/
│   └── fragments/
├── 07-memory/
│   ├── memory-index.md
│   ├── canon-facts.md
│   ├── timeline.md
│   ├── plot-threads.md
│   ├── series-state.md
│   ├── consistency-ledger.md
│   ├── memory-stream.md
│   └── deep-memory.md
├── 08-review/
│   ├── continuity-reports/
│   ├── revision-notes/
│   └── risk-checks/
└── 09-archive/
    ├── discarded-ideas.md
    └── retired-retcons.md
```

## 二、每个文件夹放什么

### `00-brief`

只放最上层项目约束：

- 一句话故事
- 目标读者
- 主体验承诺
- 禁写边界
- 平台风格约束

### `01-world`

只放世界与宇宙层资料：

- 世界规则
- 宇宙结构
- 地图体系
- 历史层和文明层

不要把人物心理或章节节奏塞进这里。

### `02-characters`

只放人物资料：

- 角色卡
- 人物小传
- 关系图
- 人物弧光
- 群像总表
- 群像弧光矩阵
- 角色身份树与命名账本（私人事件线、称呼边界、别名揭示）

### `03-outline`

只放全局结构：

- 总纲
- 故事线总表
- 结构网图
- 伏笔矩阵
- 势力棋盘
- 多层阴谋局网（R 模式：节点、因果边、线索、知识边界、反制与回收）
- 并发布局线程板（多伏笔、多局、筛选与现实代价的互相牵引）

### `04-volumes`

一卷一个文件，避免整部长篇所有卷混在一个大纲里。

### `05-chapters`

分成四层：

- `chapter-cards/`：控制单章目标
- `detailed-outlines/`：控制场景级推进
- `chapter-summaries/`：用于续写前回忆
- `gate-logs/`：存放每章的执行日志和 Checklist（如 `CH01-gate-log.md`），不嵌入正文

### `06-drafts`

只放正文，不放设定和状态说明。

### `07-memory`

这是整部作品最关键的目录。

至少应长期维护：

- 记忆索引
- canon 事实
- 时间线
- 伏笔线索
- 连载状态
- 连续性台账

### `08-review`

只放审校产物：

- 连续性问题报告
- 风格修订建议
- 风险检查单

### `09-archive`

用来放废案、弃用设定、已退役 retcon，不让它们混入当前 canon。

## 三、长篇默认骨架

如果用户要求“初步生成项目结构”“先帮我建一套小说目录”“先整理长篇工作台”，默认应先给出下面 6 个骨架目录：

1. `00-brief/`
2. `01-world/`
3. `02-characters/`
4. `03-outline/`
5. `05-chapters/`
6. `07-memory/`

原因很简单：

- `01-world` 不先落地，世界规则、地图和文明层容易被挤进总纲或正文里
- `02-characters` 不先落地，人物卡、人设、小传和群像资料就会散落在章节说明里

只有在以下情况，才允许初次整理时暂缓 `01-world` 或 `02-characters`：

- 用户明确只写短篇试稿
- 用户当前只要一个极简脑暴框架
- 用户明确说暂时不做世界观或人物档案

## 四、最小必备文件包

如果用户不想一开始建太多文件，至少先建这些：

1. `00-brief/project-brief.md`
2. `01-world/worldbuilding.md`
3. `02-characters/character-cards.md`
4. `02-characters/character-biographies.md`
5. `03-outline/master-outline.md`
6. `05-chapters/chapter-cards/`
7. `07-memory/memory-index.md`
8. `07-memory/canon-facts.md`
9. `07-memory/timeline.md`
10. `07-memory/consistency-ledger.md`

没有这 10 项，长篇中后期很容易开始乱。

如果作品采用群像推进，再至少补两项：

11. `02-characters/ensemble-cast.md`
12. `02-characters/ensemble-arc-matrix.md`

如果角色、群像或起名是作品核心，再补一项：

13. `02-characters/character-identity-tree.md`（身份树、私人事件线与命名账本）

如果作品采用总阴谋、连环布局或幕后棋局，再补一项：

14. `03-outline/conspiracy-weave.md`（与结构网图、伏笔矩阵互补；详见 `conspiracy-weaving-protocol.md`）

如果作品有多条伏笔、多局同时运行、候选人筛选或底层代价线，再补一项：

15. `03-outline/concurrent-plot-weave.md`（线程并发、遮蔽、人物多重位置与场景交通）

## 四点五、章节文件命名规则

章节相关文件默认统一使用：

- `01-第一章.md`
- `02-第二章.md`
- `03-第三章.md`

不建议默认使用：

- `001.md`
- `chapter-01.md`
- `第1章.md`

推荐原因：

1. 前缀数字方便排序
2. 中文章名方便肉眼识别
3. 在章节卡、细纲、正文草稿、章节摘要之间可以保持一致映射

推荐对应关系：

- `05-chapters/chapter-cards/01-第一章.md`
- `05-chapters/detailed-outlines/01-第一章.md`
- `05-chapters/chapter-summaries/01-第一章.md`
- `06-drafts/volume-01/01-第一章.md`

如果已经写到一百章以后，继续按两位或三位数字统一补零，但正文名仍保持“数字前缀 + 中文章序”：

- `99-第九十九章.md`
- `100-第一百章.md`

## 五、每次写作前的读取顺序

建议固定为：

1. `07-memory/memory-index.md`
2. `07-memory/canon-facts.md`
3. `07-memory/consistency-ledger.md`
4. `07-memory/timeline.md`
5. 当前卷纲
6. 当前细纲或章节卡
7. 最近章节摘要

如果写作前不按顺序读取，最容易出现：

- 角色说话口吻漂移
- 设定被偷偷改掉
- 伏笔方向被写断
- 世界规则前后不一致

## 六、输出规则

当用户要求“帮我整理小说项目”时，优先输出：

1. 推荐目录树
2. 当前阶段必须创建的文件清单
3. 文件之间的读取顺序
4. 当前阶段暂时不需要创建的文件

如果涉及章节目录或自动生成章节文件名，默认按 `01-第一章.md` 这种格式输出，不使用纯数字文件名。

不要一上来把所有文件都塞满，而是按阶段建目录、按需要补内容。
