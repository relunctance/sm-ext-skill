---
name: sm-ext-skill
description: SM 技能索引路由器 - 接收任何 Scrum Master 任务，智能推荐最合适的 skill 并执行
version: 2.0.0
hermes:
  auto_route: true
---

# SM Ext Skill - 智能技能路由器

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

### 一句话触发规则

```
任务包含...         → 直接路由到...
────────────────────────────────────────────────────────────
"sprint"、"规划"、"会议" → bmad-sm
"需求"、"po"、"product owner" → bmad-po
"epic" → pac-create-epic
"ticket"、"任务" → pac-create-ticket
"里程碑"、"milestone" → milestone-tracker
"健康"、"health" → project-health-check
"预测"、"timeline"、"进度" → project-timeline-simulator
"prd"、"需求文档" → create-prd
"prp"、"产品需求" → create-prp
"feature"、"特性" → create-feature
```

## 🔀 智能路由决策树

```
收到 SM 任务
    │
    ├─ 包含 "sprint" / "规划" / "会议"
    │   └─→ bmad-sm
    │
    ├─ 包含 "需求" / "po" / "product owner"
    │   └─→ bmad-po
    │
    ├─ 包含 "epic"
    │   └─→ pac-create-epic
    │
    ├─ 包含 "ticket" / "任务"
    │   └─→ pac-create-ticket
    │
    ├─ 包含 "里程碑" / "milestone"
    │   └─→ milestone-tracker
    │
    ├─ 包含 "健康" / "health"
    │   └─→ project-health-check
    │
    ├─ 包含 "预测" / "timeline" / "进度"
    │   └─→ project-timeline-simulator
    │
    ├─ 包含 "prd" / "需求文档"
    │   └─→ create-prd
    │
    ├─ 包含 "prp" / "产品需求"
    │   └─→ create-prp
    │
    └─ 包含 "feature" / "特性"
        └─→ create-feature
```

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-sm | Sprint规划核心：Sprint Planning、Daily Standup、Retrospective | P0 | sprint、规划、会议 |
| bmad-po | 需求质量评估：User Story、Acceptance Criteria、Definition of Ready | P0 | 需求、po、product owner |
| pac-create-epic | Epic规范化：Epic创建标准、依赖分析、价值评估 | P0 | epic |
| pac-create-ticket | Ticket规范化：Ticket模板、优先级、预估 | P0 | ticket、任务 |
| milestone-tracker | 里程碑追踪：进度监控、风险识别、报告 | P1 | 里程碑、milestone |
| project-health-check | 健康度检查：团队健康、阻塞识别 | P1 | 健康、health |
| project-timeline-simulator | 预测分析：Burndown、Sprint预测 | P1 | 预测、timeline、进度 |
| create-prd | PRD模板化：PRD文档标准化 | P2 | prd、需求文档 |
| create-prp | PRP模板化：产品需求提案 | P2 | prp、产品需求 |
| create-feature | Feature规范：Feature文档标准 | P2 | feature、特性 |

## 🎯 场景化深度参考

### 详细参考（引用）

**自然语言示例 + Fallback + 组合流** → 见 `references/quick-reference.md`

### 快速决策速查

```
Sprint 规划     → bmad-sm
需求评审        → bmad-po
Epic 创建       → pac-create-epic
Ticket 创建     → pac-create-ticket
里程碑          → milestone-tracker
健康检查        → project-health-check
进度预测        → project-timeline-simulator
PRD            → create-prd
PRP            → create-prp
Feature        → create-feature
未知任务        → bmad-sm + 询问澄清
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
│  gql-sm 主 skill                                            │
│    │                                                        │
│    ├─ 通用 SM 任务 → sm-ext-skill（路由）                    │
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

## 🚨 常见错误

### 错误 1: 过度路由

```
❌ "用户说规划 Sprint，路由到 bmad-sm，
    然后又问用户要不要用 milestone-tracker"
✓  直接路由到最可能的 skill，让用户决定是否深入
```

### 错误 2: 路由到不存在的 skill

```
❌ 根据关键词猜 skill 名称
✓  严格按照技能地图中的 skill 名称路由
```

### 错误 3: 忘记 Fallback

```
❌ 无法匹配时不知所措
✓  无法匹配时 → bmad-sm（让 SM 核心帮你判断）
```

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
