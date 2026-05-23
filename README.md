# SM Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-2.0.0-green.svg)](SKILL.md)
[![SM Skills](https://img.shields.io/badge/SM_Skills-10-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

Scrum Master 技能索引路由器 - 接收任何 SM 任务，智能推荐最合适的 skill 并执行。

## 一句话路由规则

```
收到 SM 任务
    │
    ├─ "Sprint/规划" → bmad-sm
    ├─ "需求/PO" → bmad-po
    ├─ "Epic" → pac-create-epic
    ├─ "Ticket" → pac-create-ticket
    ├─ "里程碑" → milestone-tracker
    └─ 无匹配 → bmad-sm
```

## 目录

- [快速开始](#快速开始)
- [技能地图](#技能地图)
- [工作流](#工作流)
- [升级](#升级)
- [AGENTS.md](AGENTS.md) - AI Agent 使用指南
- [INSTALL.md](INSTALL.md) - 安装部署说明

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
| bmad-sm | Sprint规划核心 | P0 |
| bmad-po | 需求质量评估 | P0 |
| pac-create-epic | Epic规范化 | P0 |
| pac-create-ticket | Ticket规范化 | P0 |
| milestone-tracker | 里程碑追踪 | P1 |
| project-health-check | 健康度检查 | P1 |
| project-timeline-simulator | 预测分析 | P1 |
| create-prd | PRD模板化 | P2 |
| create-prp | PRP模板化 | P2 |
| create-feature | Feature规范 | P2 |

## 同步到 Hermes

### 安装 Skill 文件到 Hermes Profile

```bash
# 进入仓库目录
cd /home/gql/repos/sm-ext-skill

# 执行同步脚本
bash sync-to-hermes.sh sm
```

同步后目录结构：
```
~/.hermes/profiles/sm/skills/
├── sm-ext-skill/                       # 路由器
│   ├── SKILL.md
│   └── references/
├── bmad-sm/                           # 独立 skill（软链接）
│   └── SKILL.md → sm-ext-skill/references/bmad-sm.md
├── bmad-po/                           # 独立 skill（软链接）
│   └── SKILL.md → sm-ext-skill/references/bmad-po.md
└── ...                                # 其他 skills
```

### 验证安装

```bash
# 查看已安装的 skills
ls -la ~/.hermes/profiles/sm/skills/

# 验证软链接
readlink -f ~/.hermes/profiles/sm/skills/bmad-sm/SKILL.md
```

---

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
