# LLM-Wiki Skill for WorkBuddy

> 基于 [Karpathy 的 LLM-Wiki 理念](https://gist.github.com/karpathy/b7d02a5d7ef66b1ca4bc7e4f7e82c9e3)，让 AI 持续维护你的个人知识库。

## 这是什么

一个 [WorkBuddy](https://www.codebuddy.cn/) Skill，将 LLM 变成你的知识库管理员。你提供资料，AI 自动"编译"成结构化的 Markdown Wiki，持续累积、互相链接、自动维护。

**核心理念**：知识"编译一次，持续累积"，而不是每次查询从零检索。

**多知识库架构**：支持创建多个独立命名的知识库，各自维护三层结构，互不干扰。通过 `@名称` 语法指定操作目标。

## 架构总览

### Hub 注册中心 + 独立知识库

```
~/WorkBuddy/llm-wiki/                     ← 知识库注册中心（Hub）/ Obsidian Vault 根
├── .obsidian/                  ← Obsidian 配置（全局，所有知识库共享）
├── REGISTRY.md                 ← 知识库注册表（"户口本"）
├── AI知识库/                    ← 知识库 "AI知识库"（完全独立）
│   ├── raw/
│   │   └── assets/
│   ├── wiki/
│   │   ├── sources/
│   │   ├── entities/
│   │   ├── concepts/
│   │   ├── comparisons/
│   │   └── analyses/
│   └── SCHEMA.md               ← 该知识库的身份证（Wiki Name + Wiki Root）
├── 职业规划/                    ← 知识库 "职业规划"（完全独立）
│   ├── raw/ ...
│   ├── wiki/ ...
│   └── SCHEMA.md
└── ...
```

### 每个知识库的三层结构

```
<知识库>/
├── raw/          ← 原始资料（只读，你管）
├── wiki/         ← Wiki 知识层（AI 全权维护）
│   ├── sources/  ← 资料摘要
│   ├── entities/ ← 实体页（人物、公司、产品）
│   ├── concepts/ ← 概念页（理论、方法、框架）
│   ├── comparisons/ ← 对比分析
│   └── analyses/ ← 深度分析
└── SCHEMA.md     ← 结构规范（含 Wiki Name 和 Wiki Root）
```

### 关键设计

| 组件 | 位置 | 作用 |
|------|------|------|
| REGISTRY.md | Hub 根目录 | "户口本"，记录所有知识库的名称、路径、统计 |
| SCHEMA.md | 每个知识库根目录 | "身份证"，定义该库的结构规范 |
| .obsidian/ | Hub 根目录 | Obsidian Vault 配置，所有知识库共享 |

## 安装

### 前置条件

- [WorkBuddy](https://www.codebuddy.cn/)（AI 编程助手）
- [Obsidian](https://obsidian.md/)（推荐，用于浏览 Wiki）
- Git（可选，用于版本管理）

### 安装步骤

1. 将本仓库克隆到 WorkBuddy 的 Skill 目录：

```bash
# 克隆到用户级 Skill 目录
git clone https://github.com/DoctorJerry/llm-wiki-skill.git ~/.workbuddy/skills/llm-wiki
```

2. 安装后目录结构应为：

```
~/.workbuddy/skills/llm-wiki/
├── SKILL.md              ← Skill 主文件
├── README.md             ← 本文件
├── references/
│   └── schema.md         ← Schema 参考模板
└── assets/
    ├── source-template.md
    ├── entity-template.md
    ├── concept-template.md
    ├── comparison-template.md
    └── analysis-template.md
```

3. 重启 WorkBuddy 或开始新会话

## 使用方法

### 首次使用：初始化知识库

```
#初始化wiki @AI知识库
```

AI 会创建名为 "AI知识库" 的独立知识库，自动建立目录结构和注册信息。完成后即可开始使用。

### 日常使用

| 触发词 | 功能 | 示例 |
|--------|------|------|
| `#初始化wiki` | 创建新知识库 | `#初始化wiki @AI产品` |
| `#摄入` | 处理新资料 | `#摄入 @AI知识库 C:/Downloads/article.pdf` |
| `#查询` | 查询知识 | `#查询 @AI知识库 RAG 和 LLM-Wiki 的区别` |
| `#体检` | 健康检查 | `#体检 @AI知识库` |
| `#概览` | 查看状态 | `#概览 @AI知识库` |
| `#列表` | 列出所有知识库 | `#列表` |

也支持自然语言："处理这篇"、"把这篇加入知识库"、"问一下知识库"、"创建知识库"、"列出知识库"等。

### `@名称` 语法规则

| 场景 | 行为 |
|------|------|
| 指定了 `@名称` | 直接定位该知识库 |
| 未指定 `@名称`，仅有1个知识库 | 自动使用该知识库 |
| 未指定 `@名称`，有多个知识库 | 展示编号列表供选择 |
| `@名称` 对应的知识库不存在 | 提示先 `#初始化wiki @名称` |

多库未指定时，AI 会展示如下友好提示：

```
📋 当前有 2 个知识库，请指定操作目标：

| # | 名称 | 页面数 | 最后操作 |
|---|------|--------|----------|
| 1 | AI知识库 | 12 | 2026-04-30 摄入 |
| 2 | 职业规划 | 8 | 2026-04-29 查询 |

💡 用法示例：
  #摄入 @AI知识库 xxx.pdf
  #查询 @职业规划 985学生特点
```

### 搭配工具（可选）

| 工具 | 用途 | 必需？ |
|------|------|--------|
| [Obsidian](https://obsidian.md/) | 浏览 Wiki、图谱视图 | 推荐 |
| Obsidian Web Clipper | 网页剪藏为 Markdown | 可选 |
| Git | 版本管理、历史回退 | 可选 |
| [Marp](https://marp.app/) | 从 Wiki 内容生成 PPT | 可选 |

## 工作原理

```
你提供资料 → AI 阅读 + 提取 → 生成/更新 Wiki 页面 → 索引自动更新
                                  ↓
                            实体页、概念页、对比页
                            互相链接、持续累积
                                  ↓
                            查询时直接从 Wiki 综合
                            不需要重新检索原始资料
```

**与 RAG 的区别**：RAG 每次查询都从原始文档重新检索和拼凑。LLM-Wiki 把知识"预编译"成结构化 Wiki，查询时直接综合已有页面，知识随时间复利增长。

## 致谢

- [Andrej Karpathy](https://twitter.com/karpathy) — LLM-Wiki 理念提出者
- [WorkBuddy](https://www.codebuddy.cn/) — AI 编程助手平台

## 许可

MIT License
