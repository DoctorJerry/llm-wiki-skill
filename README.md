# LLM-Wiki Skill for WorkBuddy

> 基于 [Karpathy 的 LLM-Wiki 理念](https://gist.github.com/karpathy/b7d02a5d7ef66b1ca4bc7e4f7e82c9e3)，让 AI 持续维护你的个人知识库。

## 这是什么

一个 [WorkBuddy](https://www.codebuddy.cn/) Skill，将 LLM 变成你的知识库管理员。你提供资料，AI 自动"编译"成结构化的 Markdown Wiki，持续累积、互相链接、自动维护。

**核心理念**：知识"编译一次，持续累积"，而不是每次查询从零检索。

## 三层架构

```
你的知识库/
├── raw/          ← 原始资料（只读，你管）
├── wiki/         ← Wiki 知识层（AI 全权维护）
│   ├── sources/  ← 资料摘要
│   ├── entities/ ← 实体页（人物、公司、产品）
│   ├── concepts/ ← 概念页（理论、方法、框架）
│   ├── comparisons/ ← 对比分析
│   └── analyses/ ← 深度分析
└── SCHEMA.md     ← 结构规范
```

## 安装

### 前置条件

- [WorkBuddy](https://www.codebuddy.cn/)（AI 编程助手）
- [Obsidian](https://obsidian.md/)（推荐，用于浏览 Wiki）
- Git（可选，用于版本管理）

### 安装步骤

1. 将本仓库克隆到 WorkBuddy 的 Skill 目录：

```bash
# 方式一：克隆到用户级 Skill 目录
git clone https://github.com/DoctorJerry/llm-wiki-skill.git ~/.workbuddy/skills/llm-wiki

# 方式二：手动复制
# 将文件复制到 ~/.workbuddy/skills/llm-wiki/
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

### 首次使用：初始化

告诉 AI：

```
#初始化
```

AI 会引导你选择 Wiki 存放路径，自动创建目录结构。完成后即可开始使用。

### 日常使用

| 触发词 | 功能 | 示例 |
|--------|------|------|
| `#摄入` | 处理新资料 | `#摄入 C:/Downloads/article.pdf` |
| `#查询` | 查询知识 | `#查询 RAG 和 LLM-Wiki 的区别` |
| `#体检` | 健康检查 | `#体检` |
| `#概览` | 查看状态 | `#概览` |

也支持自然语言："处理这篇"、"把这篇加入知识库"、"问一下知识库"等。

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
