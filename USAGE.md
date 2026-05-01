# skill-xiaoshuo 使用文档

面向实际操作的使用手册。回答 8 个问题：

1. 怎么开始
2. 17 种模式（A-Q）每个怎么用
3. 六道门怎么运转
4. Scene Engine 怎么跑
5. Memory Stream / 深度记忆 / 5 类一致性 怎么落地
6. 风格保护与降 AI 味怎么实操
7. 长篇为什么必须维护状态文件
8. 在不同客户端（Claude / Cursor / Codex / VS Code）里怎么用

> **不是项目介绍** —— 项目是什么 / 整体架构请看 [README.md](./README.md)。
> **不是协议规范** —— 完整规则见 [SKILL-reference.md](./SKILL-reference.md) 与 9 大 SSOT。

---

## 目录

1. [先理解它到底怎么用](#一先理解它到底怎么用)
2. [四大场景标准流程](#二四大场景标准流程)
3. [17 种使用模式（A-Q）](#三17-种使用模式a-q)
4. [每次输入最好提供什么](#四每次输入最好提供什么)
5. [输出结果格式](#五输出结果格式)
6. [执行协议使用指南](#六执行协议使用指南)
7. [Scene Engine 与双轨输出](#七scene-engine-与双轨输出)
8. [风格保护与去 AI 化（39 项实操）](#八风格保护与去-ai-化39-项实操)
9. [Memory Stream 实操](#九memory-stream-实操)
10. [深度记忆 8 维实操](#十深度记忆-8-维实操)
11. [一致性 5 类实例（C1-C5）](#十一一致性-5-类实例c1-c5)
12. [为什么一定要维护状态文件](#十二为什么一定要维护状态文件)
13. [10 个最常见错误用法](#十三10-个最常见错误用法)
14. [不同客户端里怎么用](#十四不同客户端里怎么用)
15. [相关文件](#十五相关文件)
16. [一句话使用原则](#十六一句话使用原则)

> **不要按顺序读完**。新用户 → §一 + §二 + §三（找你要的模式）即可上手；查具体痛点 → §六 至 §十一。

---

## 一、先理解它到底怎么用

**`skill-xiaoshuo` 不是万能小说 prompt，是写作执行协议系统**。三条铁律：

| 铁律 | 含义 |
|------|------|
| 强制执行 | 规则到执行节点必须过门，模型不能选择跳过 |
| 可验证执行 | 每道门必须输出判定 + 证据 |
| 不可绕过 | 用户催促也不能跳过 |

正确用法：

```
1. 明确当前阶段
2. 只做当前阶段的任务
3. 每个正文任务过 6 道门：门0(如需) → 门1 → 门2 → 门3 → 门4 → 门5
4. 关键结果保存到状态文件（Canon / 时间线 / 章节摘要 / memory-stream / deep-memory）
5. 下一轮先读取状态再续写
```

**当成一次性生成器用** → 后面必然出现：设定漂移 / 时间线冲突 / 人物失真 / 伏笔遗忘 / AI 擅自补设定 / 重复声明（同一设定每章再介绍一遍）。

### 长篇连载额外硬规则

```
1. 前三章必须埋线 + 给爽点 + 给反转
2. 第 10-15 章必须打第一阶段高潮（不能拖）
3. 单章正文 ≥ 4300 字（默认 4300-5000）
4. 反 AI 39 项硬检（13+7+19）必须 PASS（永不降级）
5. 一致性 5 类硬检 C1-C5 必须 PASS（永不降级）
6. 章首轮换：连续 3 章不允许同手法
```

---

## 二、四大场景标准流程

### 场景 1：新建项目

```
① 一句话告诉 AI 想写什么
② 系统进入门 0 → 输出项目简报 / 主角 / 冲突 / 世界约束 / 第 1 章章节卡 / 骨架
③ 用户确认（Manual Checkpoint 触发点）
④ 自动建项目目录 + A 级 7 层状态文件 + memory-stream + deep-memory
⑤ 进门 1 写前 → 门 2 场景 → 门 3 正文 → 门 4 质检 → 门 5 回写
```

最低必建文件（按项目类型自动选）：

```
短篇（≤ 3 章）:    project-brief / character / outline / chapter-card / scene-engine
长篇连载（11-30）:  + canon-facts / timeline / memory-index / chapter-summary / quality-gate
                    + memory-stream + deep-memory（A 级 7 + 第 36 层）
多卷长篇（>30）:    + plot-architecture / foreshadow-matrix / volume-outline /
                    detailed-outline / chapter-alignment / ten-chapter-echo / consistency-ledger
群像项目:           + ensemble-cast / ensemble-arc-matrix / multi-faction-dynamics /
                    faction-board / dialogue-voice-matrix
宏大世界观:         + cosmology / world-rules / world-map / universe-map
```

详见 [memory-layer-index §三](./references/memory-layer-index.md#三按项目类型选必读层) + [template-index §三](./references/template-index.md#三必备-vs-可选按项目类型)。

### 场景 2：续写下一章

```
① 系统读 memory-stream HEAD（最近 5 章 entry）→ 200-500 tokens
② 读 canon HEAD + timeline HEAD + memory-index → 700-1000 tokens
③ 读上章 chapter-summary + chapter-alignment + ten-chapter-echo → 700 tokens
④ 读 deep-memory（活跃 motifs + 当前关系弧光 + 即将超期承诺 + must 类隐性后果）
⑤ 检查 pending_writeback=false（上章已回写）
⑥ 进门 1 → 2 → 3 → 4 → 5
⑦ 门 5 追加新 entry 到 memory-stream，更新 deep-memory 8 维
```

### 场景 3：润色现有章节

进入模式 C / N（去 AI 味）/ M（投稿润色）：

```
① 读 39 项硬检数据源 + dedup-ledger
② 全文扫 13 + 7 + 19 项命中数
③ 按优先级修复（P0 结构性 → P1 句法同质 → P2 修辞过密 → P3 段落同质 → P4 局部）
④ 重过 §六 §七 + 5 类一致性
⑤ 输出修订报告（删 N 字 / 改 M 处 / 一致性 PASS）
```

### 场景 4：连续性审校

进入模式 D：

```
① 跑 5 类一致性硬检（C1 设定 / C2 行为 / C3 知识 / C4 时空 / C5 力量）
② 输出问题列表（不重写全文）
③ 按优先级排序：C1 → C4 → C3 → C5 → C2
④ 用户决定哪些修
⑤ 修复后重跑硬检
```

---

## 三、17 种使用模式（A-Q）

### 模式 A：新建小说

**触发**：用户说"新建一本……" / "我想写一个……"

**提问示例**：
> "新建一本玄幻修仙小说，男主拥有看见时间裂缝的眼睛，仇人是宗门长老，目标是查清父亲死因。短篇，10 章左右。"

**输出**：项目简报 + 主角 + 冲突 + 世界约束 + 三幕结构 + 第 1 章章节卡 + 场景骨架 → Manual Checkpoint。

### 模式 B：继续写某一章

**触发**：用户说"写第 N 章" / "继续"

**提问示例**：
> "写第 5 章。前 4 章主角已经发现父亲死因与天枢宗有关，本章应安排他第一次正面接触宗门。约 4500 字。"

**输出**：先读 memory-stream + canon + 章节卡 → 门 1 → 门 2 → 门 3 正文 → 门 4 质检 → 门 5 回写。

### 模式 C：润色现有章节

**触发**：用户提供已有正文 + "帮我润色"

**提问示例**：
> "润色这一章。重点：节奏太平、对白同声、形容词太多。" + 粘贴正文。

**输出**：定向修订（不重写全章），保持剧情与设定不变；按 §六 §七 优先级修。

### 模式 D：连续性审校

**触发**：用户说"审校" / "找问题"

**提问示例**：
> "审校 CH3-CH8。我担心反派能力前后不一致。"

**输出**：5 类一致性扫描报告 + 修复建议（不直接重写）。

### 模式 E：网文连载

**触发**：用户说"网文" / "起点" / "知乎" / "连载"

**提问示例**：
> "网文连载，起点风格，男频玄幻，每天日更 1 章。前 3 章重点抓追更感，第 10-15 章打第一阶段高潮。"

**输出**：连载状态文件（series-state）+ 三章承诺 + 阶段高潮设计 + 章首轮换表 + 字数控制单。

### 模式 F：题材特化

**触发**：用户指定题材

**提问示例**：
> "题材特化：克苏鲁，强调禁忌带 / 接触点 / 仪式倒计时。"

**输出**：题材模板 + 13 题材世界地图/时间线特化字段（详见 [world-and-timeline-output-spec §五](./references/world-and-timeline-output-spec.md#五题材特化矩阵速查)）。

### 模式 G：大布局

**触发**：用户说"大布局" / "结构网" / "草蛇灰线"

**提问示例**：
> "建立大布局：3 主线 + 5 支线 + 2 暗线，伏笔分近回收 / 中回收 / 远回收。"

**输出**：plot-architecture + storyline-board + foreshadow-matrix + faction-board。

### 模式 H：宇宙设定

**触发**：用户说"多界" / "仙界" / "宇宙"

**提问示例**：
> "宇宙设定：凡界 / 仙界 / 神域三界。仙界 1 天 = 凡界 1 年。需要建 universe-map 和时差表。"

**输出**：cosmology + world-rules + world-map + universe-map（与 timeline 流速联动）。

### 模式 I：写法吸收

**触发**：用户说"吸收某书写法" / "学某作者"

**提问示例**：
> "吸收《诡秘之主》的高层写法长板：群像 / 信息悬疑 / 仪式感。但不要复制具体桥段。"

**输出**：长板组合 + 反拼贴验收（[anti-pastiche.md](./references/anti-pastiche.md)）。

### 模式 J：项目整理

**触发**：用户说"先帮我建项目目录"

**输出**：按 [project-organization.md](./references/project-organization.md) 输出推荐目录树 + 阶段必建文件清单。

### 模式 K：群像

**触发**：用户说"群像" / "多主角" / "团队"

**提问示例**：
> "群像项目，5 主角各有独立线，每人弧光要清晰。"

**输出**：ensemble-cast + ensemble-arc-matrix + multi-faction-dynamics + dialogue-voice-matrix（防对白同声）。

### 模式 L：未知钩子

**触发**：用户说"悬疑" / "信息差" / "未知"

**输出**：unknown-hook-matrix（主未知 / 副未知 / 阶段揭示）+ 章末钩子设计 + promise-ledger 登记。

### 模式 M：平台投稿

**触发**：用户说"投稿" / "起点 / 知乎"

**提问示例**：
> "投稿起点，前三章样章。当前 AI 痕迹：解释腔重 + 对白同声。"

**输出**：投稿润色单（按平台特化）+ 39 项反 AI 全跑 + 修订建议。

### 模式 N：去 AI 味精修

**触发**：用户说"降 AI 味" / "去 AI"

**提问示例**：
> "降 AI 味。重点：第 4 章解释腔太重 + 章首老用'他意识到'。"

**输出**：39 项硬检报告（按命中数排序）+ 优先级修复（P0 结构性 → P1 句法 → P2 修辞 → P3 段落 → P4 局部）+ 改前/改后对比。

### 模式 O：剧本/双轨输出

**触发**：用户说"剧本" / "双轨" / "screenplay"

**提问示例**：
> "把第 5 章场景骨架同时输出小说 + 标准剧本格式（双轨 O 模式）。"

**输出**：scene_id 共用 + 字段级映射（Scene Schema → 小说 / 剧本两路）+ 双轨一致性 #16/#17 校验。

### 模式 P：Idea 模式（点子反推）

**触发**：用户给一句话想法

**提问示例**：
> "我有个点子：能看到别人死亡前 10 分钟的女主。"

**输出**：项目简报 / 主角 / 冲突 / 世界约束 / 三幕结构 / 第 1 章章节卡 / 场景骨架（门 0 自动建模）。

### 模式 Q：日更一句话续写

**触发**：用户给 1 句话方向指令

**提问示例**：
> "今天这章：主角第一次进入裂缝。"

**输出**：自动读 memory-stream 最近 3 章 → 解析指令 → 章节卡 → 骨架 → 正文 → 门 4 质检 → 门 5 回写。Q 模式 Token 预算 1500 tokens。

---

## 四、每次输入最好提供什么

### 最小输入包

```
- 题材：玄幻 / 悬疑 / 言情 / 都市 / 克苏鲁 / 谍战 / 古装 / ...
- 目标读者：男频 / 女频 / 通用
- 篇幅：短篇 / 中篇 / 长篇单卷 / 多卷
- 当前阶段：新建 / 续写 / 润色 / 审校
- 已有素材（如有）：人物 / 设定 / 已写章节
- 想完成的任务：一句话说清
```

### 按项目类型追加

| 项目类型 | 追加输入 |
|---------|---------|
| 网文连载 | 平台 / 更新频率 / 单章字数范围 / 爽点类型 |
| 群像 | 角色数量 / 哪几个有独立行动线 |
| 宏大世界观 | 单世界 / 多界 / 时间流速 |
| 悬疑 | 主未知 / 阶段揭示 |
| 投稿 / 去 AI | 当前最重的 AI 痕迹 |

详见 [input-collection-guide.md](./references/input-collection-guide.md)。

### 可选：指定执行模式

```
- "用 Lite 模式跑"  → 单章试写，token 预算 2000
- "用 Pro 模式跑"   → 投稿/双轨/群像，预算 10000，全 V1-V18 校验
- "用 Q 模式"       → 日更，1500 token，自动从 memory-stream 续写
- 不说 → 默认 Standard
```

---

## 五、输出结果格式

### 正文输出位置

```
- 正文 → 06-drafts/{卷}/CH{N}-第N章.md
- gate-log → 05-chapters/gate-logs/CH{N}-gate-log.md（不嵌入正文！）
- chapter-summary → 05-chapters/chapter-summaries/CH{N}-第N章.md
- memory-stream → 07-memory/memory-stream.md（追加 entry）
- deep-memory → 07-memory/deep-memory.md（更新 8 维）
```

### gate-log 模板

完整模板见 [`assets/gate-log-template.md`](./assets/gate-log-template.md)，含：

```
- 执行信息（执行模式 / 章号 / 项目规模 / 门记录 / pending_writeback）
- P0#1-#7 硬检 7 项
- AI 检测 ① - ⑬ 13 项（表面 8 + 深层 4 + 纹理 1）
- §六 形容词 7.1.1-7.1.7
- §七 AI 感扫描 8.1.1-8.1.19
- 🚨 P0#9 一致性 C1-C5
- 写作意识 #10-#15
- 读者体验评估
- 门 5 回写清单
- 记忆流 delta（含 deep + consistency 段）
```

### 本章价值声明（门 1 输出，正文前必须出现）

```
【核心体验】本章主要给读者：爽 / 痛 / 委屈 / 拉扯 / 反弹 / 暗潮
【情绪价值】本章情绪兑现点：{描述}
【推进价值】本章信息推进点：{描述}
```

### 读者体验评估（门 4 输出，正文后必须出现）

```
爽点兑现：✔/✘
信息推进：✔/✘
弃书风险：低/中/高
继续阅读理由：{一句话}
```

---

## 六、执行协议使用指南

### 6.1 六道门怎么运转

```
[门 0] Idea 解析   → 仅无结构化输入时触发；输出最小骨架
[门 1] 写前门      → 11 项检查（Canon / 时间线 / 对齐 / 反 AI 预设 / 地图时间线 / 隐性后果 / 承诺超期 / character-engine 实例化）
[门 2] 场景门      → 8 项（scene_id / 四要素 / 类型不连续 / 反预期点 / 骨架反 AI 预检）
[门 3] 正文门      → 11 项（映射完整 / Canon 无冲突 / 字数 ≥4300 / 章首 / 对话 / 群像 / 文笔质感 / 实时反 AI / dedup）
[门 4] 质检门      → 完整 P0#1-#9 + §六 7 项 + §七 19 项 + 写作意识 #10-#15 + 双轨 #16/#17
[门 5] 回写门      → gate-log + memory-stream + deep-memory + Canon / 伏笔 / 摘要更新
```

任一门 BLOCK → 自动修复（同门 1 次 / 全程 3 次）→ 仍 BLOCK → 输出报告 + 等用户决策（详见 [SKILL.md §异常 Fallback](./SKILL.md#异常与-fallback)）。

### 6.2 四级执行模式实际表现

| 模式 | 用户体验 |
|------|---------|
| **Lite** | 简洁 / 仅核心检查 / 适合试写 |
| **Standard** | 默认 / 完整 6 道门 + 完整 Checklist / 适合常规连载 |
| **Pro** | 详细 / V1-V18 全跑 / 完整证据 / 适合双轨 / 投稿 / 群像 |
| **Q** | 一句话方向指令 → 自动读 stream → 自动生成 → 适合日更 |

### 6.3 门 BLOCK 时你会看到什么

```
🚫 门 [N] BLOCK
原因：[具体规则违反]
证据：[正文段引用 / 文件冲突点 / 数据源缺失]
修复路径：
  - 自动修复尝试 1/3：[改动描述]
  - ...
仍 FAIL → 输出 BLOCK 报告 + 3 选项让你决策
```

### 6.4 pending_writeback 机制

每章门 5 必须把 6 项回写完成（章节摘要 / 时间线 / Canon / 伏笔 / 章节对齐 / 纲更新），写完才设 `pending_writeback=false`。下一章门 1 检测此值，如为 `true` → BLOCK，强制回上章过门 5。

### 6.5 Scene Engine 入口锁

请求"写正文"但**没有场景骨架** → 自动触发：

```
有章节卡 → 从章节卡生成最小骨架（2-3 场景，含 scene_id + 四要素）→ 过门 2 → 进门 3
无章节卡 → 回溯门 0（Idea 解析）→ 完整结构 → 进门 1
```

不允许"无骨架直接写正文"。

---

## 七、Scene Engine 与双轨输出

### 7.1 Pipeline 总览

```
Scene Schema (Level 1) → Mapping Layer (Level 2) → Mapping Validator (Level 3) → Quality Gate
```

- **Level 1**：23 字段数据契约（类型 / 必填 / enum / 校验规则）
- **Level 2**：字段级映射规则（Scene → 小说正文 + 剧本格式）
- **Level 3**：V1-V11 逐场景校验 + V12-V18 双轨全章校验 = 18 项
- **Quality Gate**：P0/P1/P2 三级 + §六 + §七 + P0#9

### 7.2 填写场景骨架

每场景**必填**：

```
scene_id: CH{章号}-S{序号}（如 CH05-S2）
进入状态: 进入本场景前的局势
场景目标: 本场景要达成什么
场景冲突: 谁/什么阻碍
退出状态: 离开本场景后的局势
类型: 推进 / 升级 / 蓄力 / 间歇 / 转折
驱动角色: 谁主导
预估字数: 800-2000
```

### 7.3 V1-V18 校验器

详见 [scene-schema.md](./references/scene-schema.md)：reject 类必修；warn 类标注但不阻断；pass 类通过。

### 7.4 双轨输出（O 模式）

scene_id 共用 → 字段映射两路：

```
Scene 骨架 ─┬→ 小说正文（叙事化展开）
            └→ 剧本格式（场号 / 内外 / 日夜 / 对白动作分行）
```

双轨一致性 #16/#17 校验：剧情 / 角色 / 时空必须一致；表达可不同。

---

## 八、风格保护与去 AI 化（39 项实操）

### 8.1 反 AI 39 项硬检（永不降级）

完整计算口径见 [anti-ai-thresholds.md](./references/anti-ai-thresholds.md)。

```
表面层 8（① - ⑧）：
  ① 无平均句（同段句长变化率 ≥30%）
  ② 无解释腔（禁段首心理判断）
  ③ 无抽象情绪堆叠（震惊/愤怒/绝望 必换为身体反应）
  ④ 无对白同声（≥3 角色遮名可辨）
  ⑤ 有现场感（每场景动作 + 环境互动）
  ⑥ 不过度工整（保留 ≥3 处粗粝感）
  ⑦ 情绪有失控（高压段短句/断句/不完整句）
  ⑧ 无模板感（连续 3 段首句不重复句式）

深层 4（⑨ - ⑫）：
  ⑨ 段落思维打破（≥30% 段无结论 + 段长 std ≥40%）
  ⑩ 叙事链非线性（≥2 因果断裂 + ≥1 反应偏移 + ≥1 信息遗漏）
  ⑪ 词汇去高频（禁缓缓/微微/轻轻 + 800 字 ≥1 口语碎片）
  ⑫ 信息密度波动（密段空段共存 + ≥1 大事小写）

纹理 1（⑬）：
  ⑬ 纹理验收（≥2 角色毛边 + ≥20% 素段 + ≥1 配角真实个性 + ≥1 信息弯路 + 金句 ≤4 间隔 ≥800）
```

### 8.2 §六 形容词堆砌零容忍 7 项

```
7.1.1 同名词修饰 ≤2 个
7.1.2 千字形容词 ≤30
7.1.3 类比修辞（像/如/宛如/仿佛）≤3/千字
7.1.4 程度副词（非常/极其/十分）≤2/千字
7.1.5 "XX的"链长 ≤1（禁 XX的XX的）
7.1.6 同段重复修饰 ≤1
7.1.7 形容词起首段 = 0
```

替换原则：删完用 → 精准动词 / 具体物件 / 身体反应 / 环境互动 / 直接动作。

### 8.3 §七 AI 感扫描 19 项

句法 6 + 词汇 4 + 段落 3 + 修辞 3 + 全局 3 = 19 项。详见 [anti-ai-thresholds §七](./references/anti-ai-thresholds.md#七ai-感残留全维扫描门-4-完稿后)。

### 8.4 修复优先级

```
P0 结构性指纹（必修）：排比 / 三层比喻 / 量词单调
P1 句法同质：主语开头 / 他她开头 / 完整句比例
P2 修辞过密：拟人化 / 如同句 / XX地
P3 段落同质：段尾收束 / 段首景物 / 过渡词
P4 局部修饰：了字结尾 / 抽象名词 / 的地得密度
```

每轮只攻一个 P 级，攻完重扫整章。

---

## 九、Memory Stream 实操

### 9.1 文件路径

`07-memory/memory-stream.md`（项目第 1 章门 5 回写时自动建）。

### 9.2 单 entry 格式

```markdown
## CH{N} (YYYY-MM-DDTHH:MM)

canon:
- 新增：{条目描述}
- 修改：{条目} {old → new}

timeline:
- {时间点}｜{事件}｜地点 {地点}｜类型 {canon/major/subplot/...}

characters:
- {角色}：{状态变化描述}

foreshadow:
- {伏笔 ID}：{埋点 / 偏移 / 回收}

worldmap:
- 新地点：{名称} ({区域 / 坐标})

universemap:
- 跨界事件：{事件}
（单世界故事此段省略）

deep:
  motifs: 新出现/再现的母题
  relations: 关系弧光更新
  knowledge: 知识状态变更
  promises: 新承诺 / 兑现
  fingerprint: 本章主情绪 + 强度
  consequences: 新增隐性后果
  dedup: 本章压缩了哪些重复
  consistency:
    C1 设定: ✅
    C2 行为: ✅
    C3 知识: ✅
    C4 时空: ✅
    C5 力量: ✅
    repair_count: 0/3
```

无变化的段填 `—`。

### 9.3 续写时怎么用

```
1. 读 memory-stream HEAD（最近 5 章 entry）→ 200-500 tokens
2. 提取 delta：canon 新增 / 时间线新事件 / 知识状态变更 / 即将超期承诺
3. 与 canon HEAD + timeline HEAD 合并 → 当前最新视图
4. 不读 5 章前 chapter-summary（除非用户要求）
5. token 节省 60-80%
```

### 9.4 Token 预算（按模式）

| 模式 | A 级总预算 | 第 36 层（deep）|
|------|----------:|--------------:|
| Lite | 2000 | 200（仅 dedup-ledger）|
| Standard | 5000 | 800（8 维 HEAD）|
| Pro | 10000 | 2000（8 维 FULL）|
| Q | 1500 | 300（仅 dedup + active 承诺）|

详见 [memory-streaming-protocol §四](./references/memory-streaming-protocol.md#四token-预算按执行模式)。

---

## 十、深度记忆 8 维实操

### 10.1 文件路径

`07-memory/deep-memory.md`（每章门 5 回写时增量更新）。

### 10.2 8 维内容

| 维度 | 字段示例 |
|------|---------|
| 1 主题母题 | "裂缝" / "金属味" / "断手" 反复出现的非情节元素 |
| 2 关系弧光 | 陆沉 ↔ 李枫：戒备 → 试探 → 破冰 → 依赖（每步必有触发事件）|
| 3 知识状态图谱 | 角色 × 信息节点 × 知道/不知道 + 变更章号 |
| 4 承诺账本 | P001：父亲死因（埋点 CH1，软上限 CH11，硬上限 CH21，状态 active）|
| 5 读者体验指纹 | CH3 主情绪：拉扯，强度 4 |
| 6 风格指纹 | 平均句长 18 字 / 短句占比 22% / 章首手法分布 / 高频词 Top 10 |
| 7 隐性后果 | 反派被下毒（CH3 触发，CH3-CH18 必体现衰弱） |
| 8 dedup-ledger | "灵气体系" 首次说全 CH1，简写"灵气" |

### 10.3 Reference-once 原则（防重复声明）

| 档位 | 何时可用 | 字数预算 |
|------|---------|---------|
| 🔴 完整说全 | 首次出现 / 卷首回顾 / 重大设定改变 | 不限 |
| 🟡 半说 | 距上次 ≥ 5 章 / 关键场景需提醒 | 原版 30% |
| 🟢 简称 | 默认所有重复出现 | 原版 5% |

**强制压缩**：连续 3 章重复同一定义（≥ 30 字相同）→ 门 4 BLOCK，自动改简称。

### 10.4 Cold-start（项目前 3 章）

`deep-memory.md` 首建时多维空白，**前 3 章只 build 不 check**：
- knowledge-state 仅登记不查矛盾
- dedup 阈值放宽（连续 3 章 → 5 章）
- latent-consequences 范围尚无（影响章 ≥ +5）→ 不强制
- 风格指纹 CH6 起 baseline 才算稳

详见 [deep-memory-protocol §4.1.1](./references/deep-memory-protocol.md#411-cold-start-fallback前-3-章项目启动时)。

---

## 十一、一致性 5 类实例（C1-C5）

### C1 设定一致性

❌ FAIL：「天枢宗弟子守在门外」（CH3 写"天枢宗"，CH8 写"天极宗"）
✅ 修复：改"天极宗" → "天枢宗"，更新 dedup-ledger 标准简称。

### C2 行为一致性

❌ FAIL：主角动机"为父复仇"，CH12 在没有任何触发事件下原谅仇人。
✅ 修复：添加触发事件（仇人为救主角妹妹身亡 / 主角发现仇人也是被骗）让转变有铺垫。

### C3 知识一致性

❌ FAIL：CH8 陆沉已确认李枫真实身份，CH12「陆沉震惊地看着李枫，'原来你是……'」
✅ 修复：改"震惊" → "果然"；或在 CH9 补"陆沉装作不知"明示。

### C4 时空一致性

❌ FAIL：上章结尾"夜里"，本章开头"清晨"但事件紧接（漏掉一夜）。
✅ 修复：加时间过渡段（"一夜过去"）或调整事件起点。

### C5 力量一致性

❌ FAIL：主角筑基期，本章无突破直接打元婴层级反派。
✅ 修复：CH 前补突破事件，或改主角"靠秘宝/伏击/反派配合"取胜。

详见 [consistency-enforcement-protocol §二](./references/consistency-enforcement-protocol.md)。

---

## 十二、为什么一定要维护状态文件

```
不维护 → 30 章后必然出现：
  • 角色名漂移（"陆沉" / "陆尘" 混用）
  • 时间线倒错（事件时间和上下文不符）
  • 伏笔遗忘（CH3 埋的钩子 CH40 没人记得）
  • 知识矛盾（角色对已知节点又表震惊）
  • 力量滑坡（反派一会儿一掌劈山一会儿被普通人打趴）
  • 重复声明（每章都重新介绍灵气是什么）
  • AI 擅自补设定（漏写的细节模型自己脑补）

维护 → 每章成本：
  • gate-log 写盘（自动）
  • memory-stream 追加 entry（自动，~50 行）
  • deep-memory 8 维更新（自动）
  • Canon / 时间线 / 伏笔追加（每章 ≤ 5 条新增）
  Total: 新增 ~200 字 / 章

收益：
  • 50 章后 token 加载量不随章节数线性膨胀（≤ 30% 增长）
  • 0 设定漂移 / 0 时间线冲突 / 0 知识矛盾
  • 续写时上下文恢复 < 5 秒
```

---

## 十三、10 个最常见错误用法

| # | 错误 | 后果 | 正确做法 |
|---|------|------|---------|
| 1 | 一上来写整本书 | 后期必崩 | 分阶段，每章过 6 道门 |
| 2 | 没章节卡直接写正文 | 门 2 BLOCK | 先填 chapter-card |
| 3 | 写完不回写状态 | pending_writeback 累积 BLOCK | 门 5 必跑 |
| 4 | "帮我续写"无约束 | 门 1 BLOCK 数据源缺失 | 提供章号 + 重点 |
| 5 | 边写边审校 | 模型出戏 / 节奏乱 | 写完一章再过门 4 |
| 6 | 门检查只写"通过"无证据 | 协议 1 视为假执行 | 必带证据 |
| 7 | 跳过场景骨架直接写正文 | 协议 2.1 入口锁触发 | 先过门 2 |
| 8 | 连续跳回写 | 多章 pending_writeback 堆积 → 不可恢复 | 一章一过门 5 |
| 9 | 有完整结构还强制门 0 | 浪费 token | 让模型自动判断 |
| 10 | 只追结构正确忽视体验 | 协议优先级颠倒 | 可读性 > 情绪价值 > 推进 > 人物 > 结构 |

---

## 十四、不同客户端里怎么用

### 14.1 Claude Code / Claude（推荐）

完整支持 SKILL.md skill loader：

```
1. 把项目克隆到本地
2. cd 到 skill-xiaoshuo 目录
3. 直接对话："新建一本……" / "写第 N 章"
4. 系统自动读 SKILL.md → 进入对应模式
```

### 14.2 Codex / Cursor / VS Code

部分支持 skill 概念，建议手动加载：

```
1. 复制 SKILL.md 内容粘贴到上下文
2. 或者打开项目让 IDE 看到 SKILL.md
3. 提问时显式提"按 SKILL.md 协议执行"
```

### 14.3 通用 LLM（无 skill 支持）

```
1. 把 SKILL.md + 当前任务相关的 1-2 个 SSOT 粘贴到 system prompt
2. 例：续写场景粘贴 anti-ai-thresholds + memory-streaming-protocol
3. 用户提问时模型仍能按协议执行
```

### 14.4 Obsidian（仓库浏览）

skill-xiaoshuo 兼容 Obsidian Markdown：

```
- 用 Obsidian 打开整个项目
- wiki link 可点击跳转
- canon-facts / timeline / chapter-summary 用 Obsidian 反向链接管理
- 但门检查需要 LLM 跑，Obsidian 本身只做浏览
```

---

## 十五、相关文件

- [README.md](./README.md) — 项目门面 + 系统架构
- [SKILL.md](./SKILL.md) — 精简运行时版（~230 行），每次调用加载
- [SKILL-reference.md](./SKILL-reference.md) — 完整参考手册（~2600 行），按需加载

**9 大 SSOT**：
- [anti-ai-thresholds.md](./references/anti-ai-thresholds.md) — 反 AI 39 项计算口径
- [world-and-timeline-output-spec.md](./references/world-and-timeline-output-spec.md) — 13 题材闭环
- [memory-streaming-protocol.md](./references/memory-streaming-protocol.md) — Hot/Warm/Cold + 预算 + DELTA
- [memory-layer-index.md](./references/memory-layer-index.md) — 35 层"用哪层"
- [consistency-enforcement-protocol.md](./references/consistency-enforcement-protocol.md) — 5 类一致性
- [deep-memory-protocol.md](./references/deep-memory-protocol.md) — 第 36 层 8 维
- [template-index.md](./references/template-index.md) — 60 模板导航
- [closure-map.md](./references/closure-map.md) — 全协议闭环验证矩阵
- [gate-log-template.md](./assets/gate-log-template.md) — gate-log 输出格式

**专题参考**：[references/](./references/)（33 份，含 input-collection-guide / first-sentence-de-ai / scene-schema 等）

**模板**：[assets/](./assets/)（60 套，按 [template-index](./references/template-index.md) 选用）

**示例**：[examples/](./examples/)（10 个示例项目）

---

## 十六、一句话使用原则

**用户输入一句话 → 系统自动建模 → 强制 6 道门串行 → 39 项反 AI + 5 类一致性 + 36 层记忆 + Reference-once dedup → 正文输出 + memory-stream 追加 + deep-memory 更新 → 下一章。不允许跳门、不允许漏 dedup、不允许写错。**
