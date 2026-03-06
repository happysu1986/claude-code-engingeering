# 示例项目：参考型 Skill

这个项目展示最简单的**参考型 Skill** 结构——封装企业 API 设计规范，让 Claude 在编写或审查 API 代码时自动加载。

## 目录结构

```
01-reference-skill/
└── .claude/skills/api-conventions/
    └── SKILL.md          # 唯一必需的文件
```

## Skill 说明

这是一个 API 设计规范 Skill。当用户编写或讨论 API 相关代码时，Claude 会自动识别并加载这份规范知识。

## 参考型 Skill 的关键特征

### 1. 提供知识，而非执行步骤

参考型 Skill 的内容是**规范和约定**（"URL 用复数名词"、"响应格式统一为 data/error/meta"），不是任务流程（"第一步做X，第二步做Y"）。

### 2. Claude 自动触发

没有设置 `disable-model-invocation: true`，Claude 通过语义匹配 description 自动判断何时需要加载。

### 3. 有效的 description

```yaml
description: API design patterns and conventions for this project. Covers RESTful URL naming, response format standards, error handling, and authentication requirements. Use when writing or reviewing API endpoints, designing new APIs, or making decisions about request/response formats.
```

这个 description 之所以有效：
- 说明了 Skill 提供什么知识（API design patterns and conventions）
- 列出了具体覆盖的领域（URL naming, response format, error handling, authentication）
- 包含了触发场景（writing or reviewing API endpoints, designing new APIs）

### 4. 只读工具限制

```yaml
allowed-tools:
  - Read
  - Grep
  - Glob
```

规范查阅不需要修改代码，只读权限足矣。

## 测试方法

在 Claude Code 中，尝试以下请求：
- "帮我写一个用户管理的 API 接口"
- "设计一个订单查询的 REST API"
- "这个 API 的响应格式对吗？"

Claude 应该会自动加载 api-conventions Skill，按照项目规范来编写或审查 API。

## 与企业本体论的关联

这个 Skill 体现了 Skills 作为"企业本体论载体"的核心理念：
- 它封装的是**企业的"做事方式"**——API 应该长什么样
- 它不是被动文档，而是 Claude 可以自动发现、理解、应用的**可操作知识**
- 它对应企业 SOP 体系中的《API 设计规范手册》

## 学习要点

1. **参考型 vs 任务型**：参考型提供知识供 Claude 应用；任务型提供执行步骤让 Claude 操作
2. **description 是触发器**：写得越具体，触发越准确
3. **allowed-tools 增强安全**：限制工具防止意外操作
4. **Skill 最小化**：一个 SKILL.md 就是完整的 Skill
