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
│   └── ensemble-arc-matrix.md
├── 03-outline/
│   ├── master-outline.md
│   ├── storyline-board.md
│   ├── plot-architecture.md
│   ├── foreshadow-matrix.md
│   └── faction-board.md
├── 04-volumes/
│   ├── volume-01-outline.md
│   ├── volume-02-outline.md
│   └── ...
├── 05-chapters/
│   ├── chapter-cards/
│   ├── detailed-outlines/
│   └── chapter-summaries/
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
│   └── consistency-ledger.md
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

### `03-outline`

只放全局结构：

- 总纲
- 故事线总表
- 结构网图
- 伏笔矩阵
- 势力棋盘

### `04-volumes`

一卷一个文件，避免整部长篇所有卷混在一个大纲里。

### `05-chapters`

分成三层：

- `chapter-cards/`：控制单章目标
- `detailed-outlines/`：控制场景级推进
- `chapter-summaries/`：用于续写前回忆

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

## 三、最小必备包

如果用户不想一开始建太多文件，至少先建这些：

1. `00-brief/project-brief.md`
2. `03-outline/master-outline.md`
3. `05-chapters/chapter-cards/`
4. `07-memory/memory-index.md`
5. `07-memory/canon-facts.md`
6. `07-memory/timeline.md`
7. `07-memory/consistency-ledger.md`

没有这 7 项，长篇中后期很容易开始乱。

如果作品采用群像推进，再至少补两项：

8. `02-characters/ensemble-cast.md`
9. `02-characters/ensemble-arc-matrix.md`

## 四、每次写作前的读取顺序

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

## 五、输出规则

当用户要求“帮我整理小说项目”时，优先输出：

1. 推荐目录树
2. 当前阶段必须创建的文件清单
3. 文件之间的读取顺序
4. 当前阶段暂时不需要创建的文件

不要一上来把所有文件都塞满，而是按阶段建目录、按需要补内容。