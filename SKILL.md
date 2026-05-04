---
name: harness-engineering
description: 在新建编码项目、设计 Agent 执行框架、或评估现有项目的 AI 协作环境时使用。涵盖三层方法论:AGENTS.md(项目规章)、工具权限设计(能力边界)、Planner-Generator-Evaluator 工作流(行为流程)。触发关键词:「新建项目要不要写 AGENTS.md」「这个 Agent 该用什么工作流」「为什么模型在这个项目里老犯错」「设计一下 Agent 的能力边界」「让 Agent 自己写约束」。
---

# Harness Engineering 方法论

## 何时调用

**应该调用:**
- 新建一个 AI 协作的编码项目,需要决定项目级规则
- 现有项目里 Agent 频繁犯同类错误,怀疑是引导问题而非模型问题
- 设计 Agent 系统的能力边界、工作流结构
- 评估现有 Agent 系统的"框架质量"
- 团队多人协作,需要统一不同人用不同 AI 工具的项目规范

**不应该调用:**
- 单次性 prompt 调优(那是 prompt engineering,不是 harness engineering)
- 纯模型选型问题(更强的模型 ≠ 更好的 harness)
- 跟编码无关的 Agent 系统(本 skill 主要面向编码 / 工具调用类 Agent)

---

## 核心论断

> **Agent 做不好任务时,问题大概率不在模型本身,而在于人类给它的执行环境(harness)设计得不够好。** 解决方向不是堆更强的基模,而是做更好的 harness。

---

## 三层框架

```
Layer 1: AGENTS.md          → 项目级规章制度,长期生效
Layer 2: 工具权限设计        → 用减法定义 Agent 能力边界
Layer 3: 工作流(可选)       → 复杂任务拆角色,简单任务跳过
```

**三层不是必须全用——按项目规模选择:**
- 个人小项目:只做 Layer 1 通常够了
- 团队中型项目:Layer 1 + Layer 2
- 复杂 Agent 系统:三层都做
- **永远先做 Layer 1**——投入产出比最高

---

## Layer 1:AGENTS.md(详见 `AGENTS_template.md`)

**是什么:** 给 AI 看的 README。已成为开源标准(Linux 基金会下属 Agentic AI Foundation 维护),Claude Code、Cursor、Codex、Copilot 等主流工具全支持。文件名固定大写 `AGENTS.md`,放项目根目录。

**跟 CLAUDE.md / .cursorrules 的关系:** AGENTS.md 是跨工具开源标准,优先用它。私有文件名可作为补充,推荐用软链统一(`ln -s AGENTS.md CLAUDE.md`)。

**核心原则:** 写给 AI 看的不是 README——删掉所有"为人类可读性服务"的内容。

**三个反模式:**
1. 把 README 复制成 AGENTS.md(信号被稀释)
2. 写得太长(超过 600 行该考虑拆分)
3. 写抽象原则不写可执行指令(模型对可验证规则响应远好于价值观)

---

## Layer 2:工具权限设计(详见 `tool-permissions.md`)

**核心思想:** 用减法定义 Agent。不要列举它能做什么,要圈定它不能做什么。

**五个权限层级:**

| 层级 | 例子 | 推荐策略 |
|------|------|---------|
| 读取类 | 读文件、查询、列目录 | 默认开放 |
| 执行类 | 跑测试、构建、lint | 默认开放,记录日志 |
| 本地变更类 | 写文件、git commit | 范围最小化,显式 allowlist |
| 外部影响类 | 调外部 API、发邮件 | 必须 confirmation |
| 不可逆破坏类 | deploy、删数据库 | 必须人工 approval |

**核心原则:** 默认拒绝(allowlist 而非 blocklist)、范围最小化、可逆优先、审计可追溯。

---

## Layer 3:Planner-Generator-Evaluator 工作流(详见 `workflow-patterns.md`)

**先回答最重要的问题:你真的需要这个工作流吗?**

大多数项目从"单角色 + 强约束 prompt + 工具调用"升级到"双角色(Planner + Generator)+ 结构化输出"就能解决 80% 的问题。强行加 Evaluator 通常是过度工程。

**强模型 vs 小模型的根本差异:**

| 维度 | 强模型方案 | 小模型方案 |
|------|----------|-----------|
| 角色切换 | 同一模型 system prompt 切角色 | 每个角色独立模型实例 |
| 跨角色传递 | 自然语言 plan | 强制 JSON / Tool calls |
| Plan 长度 | 一次 plan 多步 | plan 1-2 步,渐进式执行 |
| Evaluator | 可做开放式评估 | 拆成多个二元判断 + 代码聚合 |

---

## AutoHarness 思想:让模型为自己写约束

来自 DeepMind AutoHarness 论文(arxiv 2603.03329)。**核心想法:不要每次都让人类写约束代码,让模型分析失败案例自己写**。

**适用场景:** 任务有明确合法/非法边界(SQL 必须 parse、JSON 符合 schema)、失败模式可枚举、需要长期复用同一类约束。

**不适用场景:** 对/错是模糊判断、失败案例稀少、一次性探索任务。

---

## 实施 checklist(详见 `checklist.md`)

```
□ Step 1: 写 AGENTS.md 最小可用版本
   优先填: Project Context、Domain Vocabulary、Build & Test、Don't Do
□ Step 2: 评估是否需要工具权限设计
   如果 Agent 会接触不可逆操作(改 prod、删数据、发邮件),必做
□ Step 3: 评估是否需要分角色工作流
   简单项目跳过;复杂系统按 workflow-patterns.md 决策树走
□ Step 4: 跑通后,把翻车案例蒸馏成 AVOID 短句加回 AGENTS.md
□ Step 5(可选): 出现重复性约束失败时,引入 AutoHarness 模式
```

---

## AVOID 列表(整套方法论的踩坑总结)

```
AVOID 把 README.md 内容直接复制到 AGENTS.md
AVOID 在 AGENTS.md 里写抽象原则,要写可验证的具体规则
AVOID 强行让小模型用强模型的工作流(同一模型切角色)
AVOID 给 Agent 开放式 Evaluator 任务,要拆成多个二元判断
AVOID 一次 plan 多步给小模型执行,要 plan 1-2 步、渐进式执行
AVOID 把原始用户怒气直接当反馈喂给 Agent,会让模型变得过度防御
AVOID 不必要地搭三角色工作流,80% 的项目双角色就够
AVOID 一上来就追求"完美的 AGENTS.md",要先做最小版本然后迭代
```

---

## 配套文件

- `AGENTS_template.md` — 可填空的 AGENTS.md 模板,含 6 类必填内容
- `tool-permissions.md` — 工具权限设计的分级原则与实施动作
- `workflow-patterns.md` — 工作流模式详解(强/小模型分别处理)
- `checklist.md` — 新项目实施动作清单

---

## 参考来源

- AGENTS.md 开源标准:https://agents.md/
- 李宏毅 ML 2026 春季课程「Harness Engineering」一讲
- DeepMind AutoHarness 论文:arxiv 2603.03329
- EvoMap × 清华 Gene 论文(arxiv 2604.15097)的 AVOID 短句沉淀思想

---

## License

CC BY 4.0 — 开源给社区,可自由使用、修改、分发,请保留出处。
