# Scene Schema 定义

> **本文件是 Scene 的唯一数据契约（Single Source of Truth）。**
>
> - 所有 Scene 字段的类型、约束、enum 合法值、校验规则**只在本文件定义**
> - 其他文件（scene-engine-template.md、SKILL.md、README.md 等）只允许**引用或摘要**本文件，不允许自行定义或扩展字段
> - 如需修改 Schema，**只修改本文件**，其他文件的摘要随本文件同步更新
> - 本 Schema 是进入正文的前置门控：不通过 Schema，不允许写正文或剧本

## 字段定义

### 核心字段（所有模式必填）

| # | 字段 | 类型 | 必填 | 约束 | 合法值 / 格式 | 校验规则 |
|---|------|------|------|------|-------------|---------|
| 1 | scene_id | string | required | 全项目唯一，不可变，删除后作废不复用 | `CH{章号}-S{序号}` | 正则 `^CH\d+-S\d+$`；留空 → reject；重复 → reject |
| 2 | 场景名 | string | required | 1-20 字简明描述 | 自由文本 | 留空 → reject；超 20 字 → warn |
| 3 | POV 角色 | string | required | 必须是已建档角色名 | 角色表中的名称 | 不在角色表 → warn（待确认） |
| 4 | 地点 | string | required | 具体地名，禁止模糊词 | 自由文本 | 含"某地/某处/一个地方" → reject |
| 5 | 时间 | string | required | 具体时间点或相对时间 | 自由文本 | 与上一场景时间线矛盾 → reject |
| 6 | 类型 | enum | required | 必须在合法值内 | `行动` `反应` `过渡` `喘息` `揭示` `爆发` | 不在 enum → reject |
| 7 | 预估字数 | number | required | 200-2000 范围 | 整数 | <200 或 >2000 → warn |
| 8 | 进入状态 | string | required | 包含处境+情绪+信息持有量 | 自由文本 | 留空 → reject；与上一场景退出状态矛盾 → reject |
| 9 | 场景目标 | string | required | 一句话说清推进什么 | 自由文本 | 留空 → reject |
| 10 | 场景冲突 | string | required | 包含阻碍者+障碍+代价 | 自由文本 | 留空 → reject（过渡场景除外，须标注原因） |
| 11 | 试错结果 | enum | required | 必须在合法值内 | `成功` `失败` `代价成功` `意外偏转` | 不在 enum → reject |
| 12 | 退出状态 | string | required | 必须与进入状态不同 | 自由文本 | 与进入状态相同 → reject |
| 13 | 信息变化.读者获得 | string | required | 本场景释放给读者的新信息 | 自由文本 | 留空 → warn |
| 14 | 信息变化.角色获得 | string | optional | — | 自由文本 | — |
| 15 | 信息变化.刻意隐藏 | string | optional | — | 自由文本 | — |
| 16 | 情绪目标 | string | required | 读者应感到什么 | 自由文本 | 留空 → reject |
| 17 | 节奏标记 | enum | required | 必须在合法值内 | `快` `中` `慢` `爆` | 不在 enum → reject |
| 18 | 驱动角色 | string | required | 已建档角色名，全章不允许全填主角 | 角色表中的名称 | 全章相同 → reject |

### 剧本兼容字段（双轨模式必填，纯小说模式 optional）

| # | 字段 | 类型 | 必填 | 约束 | 合法值 / 格式 | 校验规则 |
|---|------|------|------|------|-------------|---------|
| 19 | INT/EXT | enum | 双轨 required | — | `INT` `EXT` `INT/EXT` | 双轨模式留空 → reject |
| 20 | 场景标头 | string | 双轨 required | 标准剧本格式 | `INT/EXT. 地点 - 时间` | 格式不符 → reject |
| 21 | 视觉动作 | string | 双轨 required | 只写可见行为 | 自由文本 | 含"感到/想/意识到/内心" → reject |
| 22 | 对白核心 | string | 双轨 required | 1-3 句关键台词 | 自由文本 | 留空 → reject |
| 23 | 潜台词 | string | 双轨 required | 说的≠想的 | 自由文本 | 留空 → warn |

## Enum 合法值汇总

| Enum 字段 | 合法值 | 说明 |
|----------|--------|------|
| 类型 | 行动, 反应, 过渡, 喘息, 揭示, 爆发 | 6 种场景类型，见 scene-engine-design.md |
| 试错结果 | 成功, 失败, 代价成功, 意外偏转 | 角色尝试的结果 |
| 节奏标记 | 快, 中, 慢, 爆 | 场景节奏等级 |
| INT/EXT | INT, EXT, INT/EXT | 内景/外景/内外景 |

## 标准示例

### 合法 Scene（pass）

```yaml
scene_id: CH03-S2
场景名: 夜审嫌疑人
POV 角色: 陆沉
地点: 市局审讯室
时间: 当晚 23:00
类型: 行动
预估字数: 1200
进入状态: 陆沉刚拿到监控截图，情绪紧绷，怀疑对方有同伙但没证据
场景目标: 逼嫌疑人说出第二个人的身份
场景冲突: 嫌疑人拒绝开口，威胁"你动不了我"，陆沉面临上级催结案的压力
试错结果: 代价成功
退出状态: 嫌疑人说出了一个名字，但陆沉意识到这可能是故意的误导
信息变化.读者获得: 第二个人的名字（但读者应怀疑真伪）
信息变化.角色获得: 同上
信息变化.刻意隐藏: 嫌疑人的真实目的
情绪目标: 紧张 + 不安（胜利中的不对劲）
节奏标记: 快
驱动角色: 陆沉
```

### 不合法 Scene（reject 示例）

```yaml
# ❌ scene_id 格式错误（reject）
scene_id: 第三章场景2

# ❌ 地点使用模糊词（reject）
地点: 某个房间

# ❌ 类型不在 enum 中（reject）
类型: 对话

# ❌ 进入状态留空（reject）
进入状态:

# ❌ 退出状态与进入状态相同（reject）
进入状态: 陆沉很紧张
退出状态: 陆沉很紧张

# ❌ 试错结果不在 enum 中（reject）
试错结果: 部分成功

# ❌ 节奏标记不在 enum 中（reject）
节奏标记: 中快
```

### 待确认 Scene（warn 示例）

```yaml
# ⚠️ POV 角色不在角色表中（warn，待确认是否已建档）
POV 角色: 老周

# ⚠️ 预估字数超出范围（warn）
预估字数: 2500

# ⚠️ 信息变化.读者获得 留空（warn）
信息变化.读者获得:
```

## 校验结果分类

| 结果 | 含义 | 处置 |
|------|------|------|
| **reject** | 字段不合法 | 必须修正后才能进入下一流程。禁止带着 reject 项生成正文或剧本 |
| **warn** | 字段存疑 | 标注待确认，可继续，但必须在本轮或下轮解决 |
| **pass** | 校验通过 | 无需处理 |

## 章级校验（跨场景）

单个场景通过 Schema 后，还需执行章级校验：

| # | 校验项 | 规则 | 结果 |
|---|--------|------|------|
| C1 | scene_id 唯一性 | 本章内无重复 scene_id | 重复 → reject |
| C2 | 进入/退出状态连续性 | S(n) 退出状态与 S(n+1) 进入状态衔接 | 矛盾 → reject |
| C3 | 类型组合规则 | 不允许连续 3 个同类型；行动后必须接反应或喘息；揭示后必须接反应 | 违反 → reject |
| C4 | 驱动角色多样性 | 全章不允许所有场景驱动角色相同 | 全同 → reject |
| C5 | 信息释放覆盖 | 全章至少 1 个场景有明确信息释放 | 全部留空 → reject |
| C6 | 总字数 | 全章预估字数总和 ≥ 4300 | 不足 → warn |
| C7 | 钩子 | 最后一个场景退出状态含未解决压力 | 无钩子 → warn |

## 校验执行时机

```
填写场景骨架
  ↓
单场景 Schema 校验（逐场景，23 字段）
  ↓ 有 reject → 修正后重新校验
  ↓ 全部 pass/warn →
章级校验（7 项）
  ↓ 有 reject → 修正后重新校验
  ↓ 全部 pass/warn →
进入正文 / 剧本生成
```

Schema 校验是进入正文的前置门控。不通过 Schema，不允许写正文。

## 与其他系统的关系（Pipeline 位置）

本 Schema 是三级校验链路的第一级：

```
Scene Schema（本文件）→ Mapping Layer → Mapping Validator → Quality Gate
    写前门控              生成约束         写后校验            交稿门控
    字段合法性            字段→输出映射     映射完整性          结构+质量
    reject → 禁止写正文    强制执行         退回 → 禁止进 QG    P0 → 禁止输出
```

- **scene-engine-template.md**：引用本 Schema 执行校验，模板中的 Schema 表是本文件的摘要版（不允许自行定义字段）
- **Mapping Layer**（scene-engine-template.md § 字段映射规则）：定义每个 Schema 字段如何映射到小说/剧本输出
- **Mapping Validator**（scene-engine-template.md § 映射校验器）：校验输出是否满足 Mapping Layer 的映射要求
- **Constraint Layer**（SKILL.md § 4.0）：Schema 校验"字段合法性"，Constraint Layer 校验"内容合规性"（Canon/时间线/世界规律）
- **Quality Gate P0**：Schema reject 项如果漏过，P0 的 scene_id 和映射合规检查会兜底拦截
