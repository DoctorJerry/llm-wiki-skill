# LLM-Wiki Schema — 知识库结构规范（参考副本）

> 本文件是 Skill 的参考模板。初始化时会被复制到 Wiki 根目录作为 `SCHEMA.md`。
> 包含目录结构、命名规范、页面模板和工作流规则的完整定义。
>
> 本知识库属于 LLM-Wiki 多知识库系统中的一个独立实例。每个知识库有唯一的名称和独立的三层架构。

---

## 0. SCHEMA.md 头部格式

初始化时，在复制本文件到知识库根目录后，在文件最顶部添加以下两行：

```
> **Wiki Name**: <知识库名称>
> **Wiki Root**: <绝对路径>
```

示例：
```
> **Wiki Name**: AI产品
> **Wiki Root**: /home/user/llm-wiki/AI产品
```

---

## 1. 目录结构

```
<wiki-root>/
├── raw/                        ← 原始资料（只读，不可修改）
│   └── assets/                 ← 图片等附件
├── wiki/                       ← Wiki 知识层（LLM 全权维护）
│   ├── index.md                ← 内容索引
│   ├── log.md                  ← 操作日志
│   ├── overview.md             ← 领域总览
│   ├── sources/                ← 资料摘要页
│   ├── entities/               ← 实体页（人物、公司、产品等）
│   ├── concepts/               ← 概念页（理论、方法、框架等）
│   ├── comparisons/            ← 对比分析页
│   └── analyses/               ← 深度分析页
└── SCHEMA.md                   ← 本文件（源文件）
```

## 2. 命名规范

| 类型 | 命名规则 | 示例 |
|------|----------|------|
| 资料摘要 | `sources/YYYY-MM-DD-简短标题.md` | `sources/2026-04-28-llm-wiki-intro.md` |
| 实体页 | `entities/实体名.md` | `entities/andrej-karpathy.md` |
| 概念页 | `concepts/概念名.md` | `concepts/rag.md` |
| 对比页 | `comparisons/A-vs-B.md` | `comparisons/rag-vs-llm-wiki.md` |
| 分析页 | `analyses/YYYY-MM-DD-主题.md` | `analyses/2026-04-28-km-trends.md` |

**原则：全部 kebab-case（小写+短横线），避免特殊字符。**

## 3. YAML Frontmatter（必须包含）

```yaml
---
title: "页面标题"
type: source | entity | concept | comparison | analysis
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [标签1, 标签2]
sources: [引用的 raw 资料列表]
---
```

## 4. 资料摘要页模板（sources/）

```markdown
---
title: "资料标题"
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [标签]
sources: [raw中的文件名]
---

# 资料标题

> **来源**: 作者/出处 · **日期**: YYYY-MM-DD · **类型**: 文章/论文/视频/笔记

## 核心要点
- 要点1
- 要点2

## 详细摘要
[2-5段详细摘要]

## 关键引用
> "原文引用"

## 与其他资料的关系
- 与 [[另一篇资料]] 的关联/对比
- 涉及实体：[[实体A]]
- 涉及概念：[[概念A]]

## 待探索问题
- [ ] 问题1
```

## 5. 实体页模板（entities/）

```markdown
---
title: "实体名称"
type: entity
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [人物/公司/产品]
sources: []
---

# 实体名称

> 一句话定义

## 基本信息
| 属性 | 值 |
|------|-----|
| 类型 | 人物/公司/产品 |
| 领域 | xxx |

## 详细描述
[随资料摄入持续更新]

## 相关资料
- [[资料A]] — 提到 xxx

## 相关实体/概念
- [[实体X]]
- [[概念Y]]
```

## 6. 概念页模板（concepts/）

```markdown
---
title: "概念名称"
type: concept
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [标签]
sources: []
---

# 概念名称

> 一句话定义

## 核心解释
[2-3段本质解释]

## 关键特征
- 特征1
- 特征2

## 应用场景
- 场景1

## 与其他概念的关系
- [[概念A]] — 对比/关联

## 相关资料
- [[资料A]]
```

## 7. 日志格式（log.md）

```markdown
## [YYYY-MM-DD HH:MM] ingest | 资料标题
- 新增：[[sources/xxx]]
- 更新：[[entities/xxx]]

## [YYYY-MM-DD HH:MM] query | 问题摘要
- 涉及：[[xxx]]

## [YYYY-MM-DD HH:MM] lint | 体检
- 发现：xxx
- 修复：xxx
```
