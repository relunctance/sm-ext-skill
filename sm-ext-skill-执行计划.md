# SM Ext Skill 创建执行计划

## 目标

基于 `profile_role_skill.md` 和 `skills-catalog.md`，创建智能技能路由器仓库 `sm-ext-skill`，包含：
- ✅ 智能索引推荐功能（决策树 + 触发关键词 + 快速参考表）
- ✅ 所有 SM skill 的 references（含 TL;DR 摘要）
- ✅ 升级方案
- ✅ learns 踩坑记录

---

## Step 1: 下载 & 解压插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
mkdir -p /home/gql/tmp/codebuddy-skills
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

---

## Step 2: 确认 GitHub 仓库

仓库名：`sm-ext-skill`

```bash
gh repo view relunctance/sm-ext-skill 2>/dev/null && echo "EXISTS" || echo "NOT_EXISTS"
```

---

## Step 3: 复制 profile_role_skill.md

```bash
cp /home/gql/repos/gql-bots/shared/profile_role_skill.md /tmp/profile_role_skill.md
```

**SM 角色 skill 列表**：

| Skill | 路径 | 级别 |
|-------|------|------|
| bmad-sm | `agent-team-agile-workflow/agents/bmad-sm.md` | P0 |
| bmad-po | `agent-team-agile-workflow/agents/bmad-po.md` | P0 |
| pac-create-epic | `commands-project-task-management/commands/pac-create-epic.md` | P0 |
| pac-create-ticket | `commands-project-task-management/commands/pac-create-ticket.md` | P0 |
| milestone-tracker | `commands-project-task-management/commands/milestone-tracker.md` | P1 |
| project-health-check | `commands-project-task-management/commands/project-health-check.md` | P1 |
| project-timeline-simulator | `commands-project-task-management/commands/project-timeline-simulator.md` | P1 |
| create-prd | `commands-project-task-management/commands/create-prd.md` | P2 |
| create-prp | `commands-project-task-management/commands/create-prp.md` | P2 |
| create-feature | `commands-project-task-management/commands/create-feature.md` | P2 |

---

## Step 4: 确认 skill 路径存在

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins

for skill in \
  "agent-team-agile-workflow/agents/bmad-sm.md" \
  "agent-team-agile-workflow/agents/bmad-po.md" \
  "commands-project-task-management/commands/pac-create-epic.md" \
  "commands-project-task-management/commands/pac-create-ticket.md" \
  "commands-project-task-management/commands/milestone-tracker.md" \
  "commands-project-task-management/commands/project-health-check.md" \
  "commands-project-task-management/commands/project-timeline-simulator.md" \
  "commands-project-task-management/commands/create-prd.md" \
  "commands-project-task-management/commands/create-prp.md" \
  "commands-project-task-management/commands/create-feature.md"
do
  if [ -f "$PLUGINS/$skill" ] || [ -f "$BASE/$skill" ]; then
    echo "✅ $skill"
  else
    echo "❌ $skill NOT FOUND"
  fi
done
```

---

## Step 5: 阅读 sm_update.md 获取上下文

```bash
cat /home/gql/repos/gql-bots/docs/roles_skill/sm_update.md
```

---

## Step 6: 阅读 skills-catalog.md 理解技能地图

**SM 技能地图**：

| 场景 | 推荐 Skill | 理由 |
|------|-----------|------|
| Sprint 规划 | bmad-sm | Sprint 规划核心 |
| 需求质量评估 | bmad-po | 需求质量评估 |
| Epic 规范化 | pac-create-epic | Epic 规范化 |
| Ticket 规范化 | pac-create-ticket | Ticket 规范化 |
| 里程碑追踪 | milestone-tracker | 里程碑追踪 |
| 健康度检查 | project-health-check | 健康度检查 |
| 预测分析 | project-timeline-simulator | 预测分析 |
| PRD 模板化 | create-prd | PRD 模板化 |
| PRP 模板化 | create-prp | PRP 模板化 |
| Feature 规范 | create-feature | Feature 规范 |

---

## Step 7: 创建 sm-ext-skill 仓库

### 7.0 仓库初始化（重要！）

```bash
# 1. 创建 Gitee 仓库
curl -X POST "https://gitee.com/api/v5/user/repos" \
  -d "name=sm-ext-skill&description=SM技能索引路由器&private=false&auto_init=false" \
  -H "Authorization: token 4995bfdbb1093963081f117438cc9b3a"

# 2. 创建本地仓库
mkdir -p /home/gql/repos/sm-ext-skill
cd /home/gql/repos/sm-ext-skill
git init
git remote add origin https://gitee.com/ztanfo_admin/sm-ext-skill.git
git remote add gitee https://ztanfo_admin:4995bfdbb1093963081f117438cc9b3a@gitee.com/ztanfo_admin/sm-ext-skill.git

# 3. 创建目录结构
mkdir -p /home/gql/repos/sm-ext-skill/learns
mkdir -p /home/gql/repos/sm-ext-skill/references
```

### 7.1 SKILL.md 设计要点

```markdown
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

## 🗣️ 自然语言触发示例（引用）

**详细示例** → 见 `references/quick-reference.md`

---

## ❓ Fallback 处理

**当任务无法匹配任何规则时**：

```markdown
1. 询问用户澄清：
   "这个任务是 Sprint 规划、需求评审、还是其他？"

2. 如果用户无法描述：
   → bmad-sm（让 SM 核心帮你判断）
```

---

## 🔗 任务组合流

```
Sprint 开始 → bmad-sm（规划）
           → pac-create-ticket（如需创建任务）
           → milestone-tracker（设置里程碑）

需求评审 → bmad-po（评估）
         → pac-create-epic（如需创建 Epic）
```

---

## 🔗 与 gql-sm 主 skill 联动

当 SM 角色加载 `sm-ext-skill` 时：

```markdown
1. 收到 SM 任务
2. 先加载 sm-ext-skill（路由器）
3. 根据任务路由到具体 skill
4. 执行完成后返回 bmad-sm 做评审
```

**注意**：`sm-ext-skill` 不会覆盖 `gql-sm` 主 skill，它们协同工作。

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

查看 `update_readme.md` 了解如何同步最新 skill。

当前版本：v2.0.0

---

### 7.2 references 复制命令

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/sm-ext-skill/references

mkdir -p $REFS

# P0 Skills
cp $PLUGINS/agent-team-agile-workflow/agents/bmad-sm.md $REFS/bmad-sm.md
cp $PLUGINS/agent-team-agile-workflow/agents/bmad-po.md $REFS/bmad-po.md
cp $BASE/commands-project-task-management/commands/pac-create-epic.md $REFS/pac-create-epic.md
cp $BASE/commands-project-task-management/commands/pac-create-ticket.md $REFS/pac-create-ticket.md

# P1 Skills
cp $BASE/commands-project-task-management/commands/milestone-tracker.md $REFS/milestone-tracker.md
cp $BASE/commands-project-task-management/commands/project-health-check.md $REFS/project-health-check.md
cp $BASE/commands-project-task-management/commands/project-timeline-simulator.md $REFS/project-timeline-simulator.md

# P2 Skills
cp $BASE/commands-project-task-management/commands/create-prd.md $REFS/create-prd.md
cp $BASE/commands-project-task-management/commands/create-prp.md $REFS/create-prp.md
cp $BASE/commands-project-task-management/commands/create-feature.md $REFS/create-feature.md
```

### 7.3 references 添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    case "$f" in
      bmad-sm.md)
        echo "<!-- TL;DR: Sprint规划核心：Sprint Planning、Daily Standup、Retrospective -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      bmad-po.md)
        echo "<!-- TL;DR: 需求质量评估：User Story、Acceptance Criteria、Definition of Ready -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      pac-create-epic.md)
        echo "<!-- TL;DR: Epic规范化：Epic创建标准、依赖分析、价值评估 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      pac-create-ticket.md)
        echo "<!-- TL;DR: Ticket规范化：Ticket模板、优先级、预估 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      milestone-tracker.md)
        echo "<!-- TL;DR: 里程碑追踪：进度监控、风险识别、报告 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      project-health-check.md)
        echo "<!-- TL;DR: 健康度检查：团队健康、阻塞识别 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      project-timeline-simulator.md)
        echo "<!-- TL;DR: 预测分析：Burndown、Sprint预测 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      create-prd.md)
        echo "<!-- TL;DR: PRD模板化：PRD文档标准化 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      create-prp.md)
        echo "<!-- TL;DR: PRP模板化：产品需求提案 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      create-feature.md)
        echo "<!-- TL;DR: Feature规范：Feature文档标准 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
    esac
  fi
done
```

---

## Step 8: 创建 learns/ 踩坑记录

```bash
mkdir -p /home/gql/repos/sm-ext-skill/learns
```

**learns/README.md**：

```markdown
# SM Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### bmad-sm / bmad-po
- **位置**: `plugins/agent-team-agile-workflow/agents/`
- **注意**: bmad 系列都在 plugins 目录

### pac-create-* / milestone-tracker / project-health-check / etc.
- **位置**: `external_plugins/commands-project-task-management/commands/`
- **注意**: 所有 pac-* 和 project-* 命令都在这个目录

## #source-区分

| 目录 | Skills |
|------|--------|
| `plugins/agent-team-agile-workflow/` | bmad-sm, bmad-po |
| `external_plugins/commands-project-task-management/` | pac-create-*, milestone-tracker, project-health-check, project-timeline-simulator, create-prd, create-prp, create-feature |
```

---

## Step 9: 创建 references/quick-reference.md

```bash
cat > /home/gql/repos/sm-ext-skill/references/quick-reference.md << 'EOF'
<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# SM 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "开始 Sprint Planning" | bmad-sm | Sprint 规划 |
| "评审需求质量" | bmad-po | PO 需求评估 |
| "创建一个 Epic" | pac-create-epic | Epic 规范 |
| "建个 Ticket" | pac-create-ticket | Ticket 规范 |
| "检查里程碑" | milestone-tracker | 里程碑追踪 |
| "项目健康吗" | project-health-check | 健康检查 |
| "预测下进度" | project-timeline-simulator | 进度预测 |
| "写 PRD 文档" | create-prd | PRD 模板 |
| "写 PRP" | create-prp | PRP 模板 |
| "写 Feature 规范" | create-feature | Feature 规范 |

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
"开始新的 Sprint"
    │
    ├─ bmad-sm（规划）
    │     ├─ pac-create-ticket（如需创建任务）
    │     └─ milestone-tracker（设置里程碑）
    │
    └─ project-health-check（如需健康检查）
```

### 组合 2: 需求评审

```
"评审需求质量"
    │
    ├─ bmad-po（评估需求）
    │     └─ pac-create-epic（如需创建 Epic）
    │
    └─ create-prd（如需文档化）
```

### 组合 3: 项目监控

```
"检查项目状态"
    │
    ├─ project-health-check（健康检查）
    │
    ├─ milestone-tracker（里程碑进度）
    │
    └─ project-timeline-simulator（预测分析）
```

---

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  任务类型              │  首选 Skill         │  辅助      │
├─────────────────────────────────────────────────────────────┤
│  Sprint 规划           │  bmad-sm            │            │
│  需求评审              │  bmad-po            │  pac-create-epic │
│  Epic 创建             │  pac-create-epic    │            │
│  Ticket 创建           │  pac-create-ticket  │            │
│  里程碑追踪            │  milestone-tracker   │            │
│  健康检查              │  project-health-check│           │
│  进度预测              │  project-timeline-s. │  milestone  │
│  PRD 文档              │  create-prd         │            │
│  PRP 文档              │  create-prp         │            │
│  Feature 规范          │  create-feature     │            │
│  未知任务              │  bmad-sm            │  询问澄清  │
└─────────────────────────────────────────────────────────────┘
```
EOF
```

---

## Step 10: 创建 update_readme.md 升级方案

```bash
cat > /home/gql/repos/sm-ext-skill/update_readme.md << 'EOF'
# SM Ext Skill 升级方案

## 执行计划

详见 `sm-ext-skill-执行计划.md`（详细步骤说明）

## 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `gql-bots/shared/profile_role_skill.md` 变化时
3. `gql-bots/shared/skills-catalog.md` 更新时

## 升级步骤

### Step 1: 下载最新插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

### Step 2: 同步 references

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/sm-ext-skill/references

# 重新复制所有 skill 文件
# [同 Step 7 的复制命令]
```

### Step 3: 重新添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    # 添加 TL;DR
    ...
  fi
done
```

### Step 4: 更新 quick-reference.md（如需要）

如果 skills-catalog.md 更新，同步更新 `references/quick-reference.md`。

### Step 5: 提交

```bash
cd /home/gql/repos/sm-ext-skill
git add -A
git commit -m "chore: sync with latest codebuddy-plugins"
git push origin main
```

## 版本号规则

| 类型 | 规则 |
|------|------|
| 主版本 | skill 索引结构变化、决策树重构 |
| 次版本 | 新增/删除 skill、触发关键词更新 |
| 修订版 | 内容更新、TL;DR 更新 |

## 路径速查

| Skill | 源路径 |
|-------|--------|
| bmad-sm | `plugins/agent-team-agile-workflow/agents/bmad-sm.md` |
| bmad-po | `plugins/agent-team-agile-workflow/agents/bmad-po.md` |
| pac-create-epic | `external_plugins/commands-project-task-management/commands/pac-create-epic.md` |
| pac-create-ticket | `external_plugins/commands-project-task-management/commands/pac-create-ticket.md` |
| milestone-tracker | `external_plugins/commands-project-task-management/commands/milestone-tracker.md` |
| project-health-check | `external_plugins/commands-project-task-management/commands/project-health-check.md` |
| project-timeline-simulator | `external_plugins/commands-project-task-management/commands/project-timeline-simulator.md` |
| create-prd | `external_plugins/commands-project-task-management/commands/create-prd.md` |
| create-prp | `external_plugins/commands-project-task-management/commands/create-prp.md` |
| create-feature | `external_plugins/commands-project-task-management/commands/create-feature.md` |
EOF
```

---

## Step 11: 创建 README.md

```bash
cat > /home/gql/repos/sm-ext-skill/README.md << 'EOF'
# SM Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-1.0.0-green.svg)](SKILL.md)
[![SM Skills](https://img.shields.io/badge/SM_Skills-10-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

Scrum Master 技能索引路由器 - 接收任何 SM 任务，智能推荐最合适的 skill 并执行。

## 目录

- [快速开始](#快速开始)
- [技能地图](#技能地图)
- [工作流](#工作流)
- [升级](#升级)

## 快速开始

```bash
# 安装
git clone https://gitee.com/ztanfo_admin/sm-ext-skill.git ~/.hermes/profiles/sm/skills/sm-ext-skill

# 使用
hermes -p sm -s sm-ext-skill
```

## 技能地图

| Skill | 说明 | 级别 |
|-------|------|------|
| bmad-sm | Sprint 规划核心 | P0 |
| bmad-po | 需求质量评估 | P0 |
| pac-create-epic | Epic 规范化 | P0 |
| pac-create-ticket | Ticket 规范化 | P0 |
| milestone-tracker | 里程碑追踪 | P1 |
| project-health-check | 健康度检查 | P1 |
| project-timeline-simulator | 预测分析 | P1 |
| create-prd | PRD 模板化 | P2 |
| create-prp | PRP 模板化 | P2 |
| create-feature | Feature 规范 | P2 |

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
EOF
```

---

## Step 12: Git 提交

```bash
cd /home/gql/repos/sm-ext-skill
git add -A
git commit -m "feat: initial sm-ext-skill with all SM skills"
git push origin main
```

---

## 验证清单

- [ ] 下载解压成功
- [ ] GitHub 仓库已创建/更新
- [ ] 所有 10 个 skill 路径验证通过
- [ ] references/ 包含所有 skill 文件（含 TL;DR）
- [ ] references/quick-reference.md 已创建
- [ ] SKILL.md 包含智能索引：
  - [ ] 一句话触发规则
  - [ ] 决策树
  - [ ] 技能地图
  - [ ] 场景化深度参考
  - [ ] Fallback 处理
  - [ ] 任务组合流
  - [ ] 主 skill 联动
- [ ] learns/ 有踩坑记录
- [ ] update_readme.md 有升级方案
- [ ] README.md 已美化
- [ ] git push 成功
