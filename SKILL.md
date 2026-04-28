---
name: llm-wiki
description: "LLM-Wiki 个人知识库管理引擎。基于 Karpathy 的 LLM-Wiki 理念，通过三层架构（Raw/Wiki/Schema）构建可复利累积的个人知识库。当用户说'处理这篇'、'#摄入'、'#查询'、'#体检'、'#概览'、'#初始化'、'帮我整理这篇文章'、'把这篇加入知识库'、'问一下知识库'、'wiki体检'、'知识库概览'、'创建知识库'、'初始化wiki'时触发。支持 Init（初始化）、Ingest（摄入资料）、Query（查询知识）、Lint（健康检查）四大核心操作。"
---

# LLM-Wiki 个人知识库管理引擎

## 概述

本 Skill 实现 Karpathy 的 LLM-Wiki 理念——让 LLM 充当知识库管理员，持续维护一个结构化、互相链接的 Markdown Wiki。知识经过一次"编译"后持续累积，而非每次查询从零检索。

## Wiki 根目录

Wiki 根目录在**初始化时确定**，记录在 Wiki 根目录的 `SCHEMA.md` 文件头部。

- 如果用户尚未初始化，优先引导执行 `#初始化`
- 如果 Wiki 已存在，从 `SCHEMA.md` 头部读取路径

## 触发词与操作映射

| 触发词 | 操作 | 说明 |
|--------|------|------|
| `#初始化` / `#init` | Init | 首次使用，创建 Wiki 目录结构 |
| `#摄入` / `#ingest` + 文件路径或内容 | Ingest | 处理新资料，更新 Wiki |
| `#查询` / `#query` + 问题 | Query | 查询 Wiki 知识 |
| `#体检` / `#lint` | Lint | Wiki 健康检查 |
| `#概览` / `#overview` | Overview | 查看 Wiki 当前状态 |
| "处理这篇"、"加入知识库"、"整理这篇文章" | Ingest | 自然语言触发 |
| "问一下知识库"、"查一下 Wiki" | Query | 自然语言触发 |
| "创建知识库"、"初始化 wiki" | Init | 自然语言触发 |

## 操作流程

详细规范见 `references/schema.md`。

### Init（首次初始化）

**触发条件**：用户首次使用，或 Wiki 目录不存在时。

1. 确认用户想创建知识库，询问存放路径（建议默认值：`~/llm-wiki/` 或用户指定）
2. 创建目录结构：
   ```
   <wiki-root>/
   ├── raw/
   │   └── assets/
   ├── wiki/
   │   ├── sources/
   │   ├── entities/
   │   ├── concepts/
   │   ├── comparisons/
   │   └── analyses/
   └── SCHEMA.md
   ```
3. 将 `references/schema.md` 的内容复制为 `<wiki-root>/SCHEMA.md`，并在文件头部写入 wiki 根目录的绝对路径：
   ```
   > **Wiki Root**: <绝对路径>
   ```
4. 初始化 `wiki/index.md`（空索引框架）
5. 初始化 `wiki/log.md`（空日志）
6. 初始化 `wiki/overview.md`（空概览）
7. **可选 — Git 初始化**：检测系统是否有 `git` 命令，如果有，在 wiki 根目录执行 `git init` 并做首次提交；如果没有，跳过此步（Git 是可选组件）
8. 向用户汇报：Wiki 创建完成，路径是什么，Git 是否启用，可以开始 `#摄入`

**Git 检测方法**：执行 `git --version`，成功则说明 Git 可用。

### Ingest（摄入资料）

1. 确认资料位置（用户提供文件路径、粘贴内容、或指定 URL）
2. 读取 `SCHEMA.md` 获取 Wiki 规范和根目录路径
3. 读取 `wiki/index.md` 了解已有知识结构
4. 读取原始资料内容（如果是 URL，先抓取内容保存到 `raw/`）
5. 生成资料摘要页 → `wiki/sources/YYYY-MM-DD-简短标题.md`
6. 识别实体 → 创建或更新 `wiki/entities/实体名.md`
7. 识别概念 → 创建或更新 `wiki/concepts/概念名.md`
8. 如有对比关系 → 创建 `wiki/comparisons/A-vs-B.md`
9. 更新 `wiki/index.md` 索引
10. 在 `wiki/log.md` 追加操作记录
11. **可选 — Git 提交**：如果 Wiki 是 Git 仓库（存在 `.git/` 目录），执行 `git add -A && git commit -m "ingest: 资料标题"`；否则跳过
12. 向用户汇报：处理了什么、新增/更新了哪些页面

### Query（查询知识）

1. 读取 `wiki/index.md` 定位相关页面
2. 深入阅读相关页面内容
3. 综合回答，使用 `[[]]` 格式标注引用来源
4. 如答案有长期价值，建议存为新分析页

### Lint（健康检查）

1. 遍历 `wiki/` 下所有页面
2. 检查：页面间矛盾、过时信息、孤立页面、缺失引用、frontmatter 完整性
3. 汇报发现的问题和修复建议
4. 用户确认后执行修复
5. 在 `log.md` 记录体检结果
6. **可选 — Git 提交**：如果 Wiki 是 Git 仓库，执行 `git add -A && git commit -m "lint: 体检结果摘要"`；否则跳过

### Overview（概览）

1. 读取 `wiki/index.md` 和 `wiki/log.md`
2. 展示：页面统计、最近操作、知识覆盖情况

## 页面模板

创建 Wiki 页面时，使用 `assets/` 目录下的模板作为起点：

| 模板文件 | 用途 | 何时使用 |
|----------|------|----------|
| `assets/source-template.md` | 资料摘要页 | 每次摄入新资料时 |
| `assets/entity-template.md` | 实体页 | 识别到新实体或更新已有实体时 |
| `assets/concept-template.md` | 概念页 | 识别到新概念或更新已有概念时 |
| `assets/comparison-template.md` | 对比页 | 发现两个实体/概念有对比价值时 |
| `assets/analysis-template.md` | 分析页 | Query 产生有价值的深度分析时 |

模板中的 `{{占位符}}` 在实际创建时替换为真实内容。

## 约束

- **永远不修改 `raw/` 中的文件**
- **每次操作后必须更新 `index.md` 和 `log.md`**
- **Git 是可选组件**：检测到 `.git/` 目录才提交，不强制要求 Git
- **frontmatter 必须完整**（title, type, created, updated, tags, sources）
- 页面间引用使用 Obsidian 双向链接格式 `[[页面路径]]`
- 如发现与已有知识矛盾，在摘要页显式标注
- **Wiki 路径不硬编码**：每次操作从 `SCHEMA.md` 头部读取，或由用户在初始化时指定
