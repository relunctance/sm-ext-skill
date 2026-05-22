<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# SM 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "开始 Sprint Planning" | bmad-sm | Sprint 规划 |
| "评审需求" | bmad-po | 需求评审 |
| "创建 Epic" | pac-create-epic | Epic 创建 |
| "创建 Ticket" | pac-create-ticket | Ticket 创建 |
| "追踪里程碑" | milestone-tracker | 里程碑 |
| "检查健康度" | project-health-check | 健康检查 |
| "预测进度" | project-timeline-simulator | 进度预测 |
| "写 PRD" | create-prd | PRD 文档 |
| "写 PRP" | create-prp | PRP 文档 |
| "写 Feature" | create-feature | Feature 文档 |

---

## 🔄 Fallback 处理

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

**Fallback 规则**：
```markdown
无匹配时 → bmad-sm → 让他判断用哪个 skill
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

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  任务类型              │  首选 Skill         │  辅助      │
├─────────────────────────────────────────────────────────────┤
│  Sprint 规划          │  bmad-sm          │            │
│  需求评审              │  bmad-po          │            │
│  Epic 创建             │  pac-create-epic  │            │
│  Ticket 创建           │  pac-create-ticket│            │
│  里程碑追踪            │  milestone-tracker  │            │
│  健康检查              │  project-health-c. │            │
│  进度预测              │  project-timeline..│            │
│  PRD 文档              │  create-prd       │            │
│  PRP 文档              │  create-prp       │            │
│  Feature 文档          │  create-feature    │            │
│  未知任务              │  bmad-sm          │  询问澄清  │
└─────────────────────────────────────────────────────────────┘
```
