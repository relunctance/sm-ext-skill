---
name: sm-ext-skill
description: SM 技能索引路由器 - 接收任何 Scrum Master 任务，智能推荐最合适的 skill 并执行
version: 2.0.0
author: relunctance
license: MIT
category: gql-bots
tags:
  - sm
  - scrum-master
  - agile
  - skill-router
  - gql-bots
  - intelligent-router
hermes:
  platforms:
    hermes: true
  auto_route: true
---

# SM Ext Skill - 智能技能路由器

> **核心定位**：SM 角色的中央路由器。任何 Scrum Master 任务进来，先查这里，再路由到具体 skill。

---

## ⚡ TL;DR 速查索引

| 你要做的事 | 直接路由 | 说明 |
|------------|---------|------|
| Sprint 规划 | bmad-sm | Planning/Daily/Retro |
| 需求评审 | bmad-po | User Story、DoR |
| 创建 Epic | pac-create-epic | Epic 规范化 |
| 创建 Ticket | pac-create-ticket | Ticket 模板 |
| 里程碑追踪 | milestone-tracker | 进度监控 |
| 健康检查 | project-health-check | 团队健康 |
| 进度预测 | project-timeline-simulator | Burndown |
| 写 PRD | create-prd | 需求文档 |
| 写 PRP | create-prp | 产品提案 |
| 写 Feature | create-feature | Feature 规范 |
| 不知道用哪个 | bmad-sm | 让它帮你判断 |

---

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "Sprint 规划" | bmad-sm | `hermes -p sm -s bmad-sm` |
| "需求评审" | bmad-po | `hermes -p sm -s bmad-po` |
| "创建 Epic" | pac-create-epic | `hermes -p sm -s pac-create-epic` |
| "创建 Ticket" | pac-create-ticket | `hermes -p sm -s pac-create-ticket` |
| "里程碑" | milestone-tracker | `hermes -p sm -s milestone-tracker` |
| "健康检查" | project-health-check | `hermes -p sm -s project-health-check` |
| "预测进度" | project-timeline-simulator | `hermes -p sm -s project-timeline-simulator` |
| "写 PRD" | create-prd | `hermes -p sm -s create-prd` |
| "写 PRP" | create-prp | `hermes -p sm -s create-prp` |
| "写 Feature" | create-feature | `hermes -p sm -s create-feature` |

### 一句话触发规则（增强版）

```
任务包含...              → 直接路由到...
────────────────────────────────────────────────────────────────────────────
# Sprint 管理
"sprint"、"规划"、"会议" → bmad-sm
"daily"、"每日站会" → bmad-sm
"retro"、"回顾" → bmad-sm
"sprint review"、"评审会" → bmad-sm

# 需求管理
"需求"、"po"、"product owner" → bmad-po
"user story"、"用户故事" → bmad-po
"DoR"、"Definition of Ready" → bmad-po
"acceptance criteria"、"验收标准" → bmad-po

# 任务创建
"epic"、"史诗" → pac-create-epic
"ticket"、"任务"、"卡" → pac-create-ticket
"子任务"、"subtask" → pac-create-ticket

# 进度管理
"里程碑"、"milestone" → milestone-tracker
"进度"、"完成率" → milestone-tracker
"风险"、"阻塞" → milestone-tracker

# 健康检查
"健康"、"health"、"团队状态" → project-health-check
"阻塞"、"blocker" → project-health-check
"燃尽图"、"burndown" → project-timeline-simulator

# 预测
"预测"、"timeline"、"进度预测" → project-timeline-simulator
"容量"、"velocity" → project-timeline-simulator
"工时"、"估算" → project-timeline-simulator

# 文档
"prd"、"需求文档"、"产品需求文档" → create-prd
"prp"、"产品需求提案" → create-prp
"feature"、"特性"、"功能" → create-feature
"用户故事"、"story" → create-feature
```

---

## 🔀 智能路由决策树

```
收到 SM 任务
    │
    ├─ 🎯 Sprint 管理？
    │   └─ bmad-sm
    │         ├─ Planning
    │         ├─ Daily Standup
    │         └─ Retrospective
    │
    ├─ 🎯 需求管理？
    │   └─ bmad-po
    │         ├─ User Story
    │         ├─ Definition of Ready
    │         └─ Acceptance Criteria
    │
    ├─ 🎯 任务创建？
    │   ├─ Epic → pac-create-epic
    │   └─ Ticket → pac-create-ticket
    │
    ├─ 🎯 进度/里程碑？
    │   └─ milestone-tracker
    │         ├─ 进度监控
    │         └─ 风险识别
    │
    ├─ 🎯 健康检查？
    │   └─ project-health-check
    │
    ├─ 🎯 预测分析？
    │   └─ project-timeline-simulator
    │         ├─ Burndown
    │         └─ Velocity
    │
    ├─ 🎯 文档创建？
    │   ├─ PRD → create-prd
    │   ├─ PRP → create-prp
    │   └─ Feature → create-feature
    │
    └─ ❓ 不知道
        └─ bmad-sm + 询问澄清
```

---

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-sm | Sprint规划核心：Planning、Daily、Retro | P0 | sprint、规划、会议 |
| bmad-po | 需求质量评估：User Story、DoR、AC | P0 | 需求、po、user story |
| pac-create-epic | Epic规范化：创建标准、依赖分析 | P0 | epic、史诗 |
| pac-create-ticket | Ticket规范化：模板、优先级、预估 | P0 | ticket、任务、卡 |
| milestone-tracker | 里程碑追踪：进度监控、风险识别 | P1 | 里程碑、进度 |
| project-health-check | 健康度检查：团队健康、阻塞识别 | P1 | 健康、health、blocker |
| project-timeline-simulator | 预测分析：Burndown、Velocity | P1 | 预测、timeline、burndown |
| create-prd | PRD模板化：需求文档标准化 | P2 | prd、需求文档 |
| create-prp | PRP模板化：产品需求提案 | P2 | prp、产品提案 |
| create-feature | Feature规范：Feature文档标准 | P2 | feature、特性 |

---

## 🎯 场景化深度参考（4大场景）

### 场景 1: Sprint 规划 📋

```
需求：开始一个新的 Sprint
    │
    ├─ bmad-sm（Sprint 规划）
    │     ├─ Sprint Planning
    │     │     → 故事点估算
    │     │     → 任务分配
    │     │
    │     ├─ Daily Standup
    │     │     → 进度同步
    │     │     → 阻塞识别
    │     │
    │     └─ Retrospective
    │           → 改进点
    │
    └─ pac-create-ticket（如需创建任务）
          → 任务拆分
          → 估算
```

### 场景 2: 需求管理 📝

```
需求：新需求来了，需要评审
    │
    ├─ bmad-po（需求评审）
    │     ├─ User Story
    │     │     → 格式检查
    │     │     → 验收标准
    │     │
    │     └─ Definition of Ready
    │           → DoR 检查清单
    │
    └─ pac-create-epic（如需创建 Epic）
          → Epic 创建
          → 依赖分析
```

### 场景 3: 项目监控 📊

```
需求：监控项目进度
    │
    ├─ milestone-tracker（里程碑）
    │     → 进度监控
    │     → 风险识别
    │     → 报告生成
    │
    ├─ project-health-check（健康检查）
    │     → 团队健康
    │     → 阻塞识别
    │
    └─ project-timeline-simulator（预测）
          → Burndown 图
          → Velocity 预测
          → 完工预测
```

### 场景 4: 文档创建 📄

```
需求：需要写 PRD/Feature 文档
    │
    ├─ PRD？
    │   └─ create-prd
    │         → PRD 模板
    │         → 需求格式
    │
    ├─ PRP？
    │   └─ create-prp
    │         → 产品提案
    │         → 价值分析
    │
    └─ Feature？
        └─ create-feature
              → Feature 模板
              → 用户故事
```

### 快速决策速查

```
┌────────────────────────────────────────────────────────────┐
│  场景              │  路由顺序                              │
├────────────────────────────────────────────────────────────┤
│  Sprint 规划       │  bmad-sm → pac-create-ticket        │
│  需求评审          │  bmad-po → pac-create-epic          │
│  Epic 创建         │  pac-create-epic                   │
│  Ticket 创建       │  pac-create-ticket                  │
│  里程碑追踪        │  milestone-tracker                  │
│  健康检查          │  project-health-check               │
│  进度预测          │  project-timeline-simulator         │
│  PRD 文档         │  create-prd                         │
│  PRP 文档         │  create-prp                         │
│  Feature 文档      │  create-feature                     │
│  未知任务          │  bmad-sm + 询问澄清                 │
└────────────────────────────────────────────────────────────┘
```

---

## ❓ Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是 Sprint 规划、需求评审、还是其他？"
    │
    └─ 如果用户无法描述：
        └─→ bmad-sm（让 SM 核心帮你判断）
```

---

## 🔗 任务组合流

### 组合 1: Sprint 开始

```
"Sprint 开始"
    │
    ├─ bmad-sm（Sprint 规划）
    │     └─ pac-create-ticket（如需创建任务）
    │
    └─ milestone-tracker（如需设置里程碑）
```

### 组合 2: 需求管理

```
"新需求来了"
    │
    ├─ bmad-po（需求评审）
    │     └─ pac-create-epic（如需创建 Epic）
    │
    └─ create-prd（如需写 PRD）
```

### 组合 3: 项目监控

```
"监控项目状态"
    │
    ├─ project-health-check（健康检查）
    ├─ milestone-tracker（里程碑）
    └─ project-timeline-simulator（预测）
```

---

## 🔗 与 gql-sm 主 skill 联动

**注意**：`sm-ext-skill` 不会覆盖 `gql-sm` 主 skill，它们协同工作。

```
┌─────────────────────────────────────────────────────────────┐
│  gql-sm 主 skill                                           │
│    │                                                        │
│    ├─ 通用 SM 任务 → sm-ext-skill（路由）                  │
│    │              └─→ 具体 skill 执行                         │
│    │                                                        │
│    └─ 特定技能任务 → 直接调用具体 skill                       │
└─────────────────────────────────────────────────────────────┘
```

**何时使用 sm-ext-skill**：
- 任务模糊，需要判断用哪个 skill
- 复杂任务需要多 skill 组合
- 不确定某个 skill 是否适用

**何时直接调用具体 skill**：
- 任务明确，比如"Sprint Planning"
- 已确定需要哪个 skill
- 只需要单个 skill

---

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 说明 |
|-------|-------|------|
| bmad-sm.md | Sprint规划核心 | Planning/Daily/Retro |
| bmad-po.md | 需求质量评估 | User Story、DoR |
| pac-create-epic.md | Epic规范化 | 创建标准、依赖 |
| pac-create-ticket.md | Ticket规范化 | 模板、优先级 |
| milestone-tracker.md | 里程碑追踪 | 进度监控 |
| project-health-check.md | 健康度检查 | 团队健康 |
| project-timeline-simulator.md | 预测分析 | Burndown |
| create-prd.md | PRD模板化 | 需求文档 |
| create-prp.md | PRP模板化 | 产品提案 |
| create-feature.md | Feature规范 | Feature 文档 |

---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说"规划" | 说明规划什么（Sprint？需求？） |
| 不验证就 claim 完成 | 先确认任务类型再路由 |
| 过度路由 | 直接路由到最可能的 skill |
| 忘记 Fallback | 无法匹配时 → bmad-sm |

---

## 🔗 相关角色联动

| 角色 | 协作场景 |
|------|---------|
| **arc** | 项目启动：sm-ext-skill 规划 → arc-ext-skill 评审技术可行性 |
| **coder** | 任务分解：sm-ext-skill 规划 → coder-ext-skill 执行开发 |
| **review** | 需求评审：sm-ext-skill 评审需求 → review-ext-skill 评审实现方案 |
| **qa** | 测试规划：sm-ext-skill 规划任务 → qa-ext-skill 评审测试策略 |

---

## 🗣️ 示例对话

### 示例 1: 路由到 bmad-sm

```
用户：开始 Sprint Planning
AI：分析：包含"Sprint"、"Planning"
     路由到 bmad-sm
     执行：Sprint 规划核心流程
```

### 示例 2: 路由到 Epic 创建

```
用户：创建一个新 Epic
AI：分析：包含"Epic"
     路由到 pac-create-epic
     执行：Epic 规范化创建
```

### 示例 3: Fallback 处理

```
用户：帮我看看这个需求
AI：分析：包含"需求"
     但无法确定具体类型
     Fallback → bmad-sm
     执行：SM 核心判断任务类型
     反馈：这个任务是"需求评审"，建议用 bmad-po
```

---

## 升级说明

查看 [update_readme.md](update_readme.md) 了解如何同步最新 skill。

当前版本：v2.0.0
