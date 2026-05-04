# Harness Engineering Skill

> 一套关于「如何为 AI 编码 Agent 设计执行环境」的方法论 Skill。
> 适用于 Claude Code、Cursor、OpenAI Codex、GitHub Copilot 等所有支持 SKILL 协议或 AGENTS.md 标准的 AI 编码工具。

## 这是什么

这套 Skill 解决一个被普遍忽视的问题:

> **当 Agent 做不好任务时,问题大概率不在模型本身,而在于人类给它的执行环境(harness)设计得不够好。** 解决方向不是堆更强的基模,而是做更好的 harness。

它把 harness 设计这件事拆成三个可操作的层次:

- **Layer 1 — AGENTS.md**:项目级规章制度,长期生效
- **Layer 2 — 工具权限设计**:用减法定义 Agent 能力边界
- **Layer 3 — 工作流模式**:复杂任务怎么拆角色,简单任务怎么不拆

每一层都给出了具体的实施动作、可填空的模板、常见反模式和 AVOID 列表,而不是停留在抽象原则。

## 适合谁用

- **个人开发者**:做副业项目时,用 Claude Code / Cursor 等 AI 编码工具,经常遇到"模型反复在同一个地方犯错"的情况
- **小团队**:多人协作时,需要统一不同人用不同 AI 工具的项目规范,避免每个人都在重复踩同一类坑
- **Agent 系统设计者**:在做严肃的 Agent 产品,需要一套关于"模型周边胶水层"的系统化思考框架
- **AI 协作的研究者**:想了解 2026 年「Harness Engineering」这个新方向到底在讲什么

## 快速开始

### 方式一:作为 Claude Skill 安装(推荐)

把整个目录放到 Claude 的 user skills 路径下:

```bash
# Claude Desktop / Claude Code 用户
cp -r harness-engineering ~/.claude/skills/user/

# 然后在对话里说:
"调用 harness-engineering skill 帮我设计一个新项目的 AGENTS.md"
```

### 方式二:直接读文档使用

如果你不用 Claude,只是想了解方法论,直接按下面顺序读:

1. **`SKILL.md`** — 主入口,先读这个建立整体认知(约 5 分钟)
2. **`checklist.md`** — 实施动作清单,告诉你按什么顺序做(约 5 分钟)
3. **`AGENTS_template.md`** — 复制模板填空,创建你项目的 AGENTS.md(约 30 分钟动手)
4. **`tool-permissions.md`** — 当 Agent 接触不可逆操作时再看
5. **`workflow-patterns.md`** — 当你确定要搭分角色工作流时再看

**建议从 30 分钟启动方案开始**(`checklist.md` 末尾)——先快速搭起最小可用版本,跑起来再迭代,而不是一次到位写到完美。

## 文件结构

```
harness-engineering/
├── README.md               # 本文件
├── SKILL.md                # 主入口,三层框架概述
├── AGENTS_template.md      # 可填空的 AGENTS.md 模板
├── tool-permissions.md     # 工具权限设计的分级原则
├── workflow-patterns.md    # 工作流模式详解(强/小模型差异)
└── checklist.md            # 实施动作清单 + 30 分钟启动方案
```

## 核心方法论

### 三层框架

```
Layer 1: AGENTS.md          → 项目级规章制度,长期生效
Layer 2: 工具权限设计        → 用减法定义 Agent 能力边界
Layer 3: 工作流(可选)       → 复杂任务拆角色,简单任务跳过
```

**关键原则:三层不是必须全用——按项目规模选择。**

- 个人小项目:只做 Layer 1 通常够了
- 团队中型项目:Layer 1 + Layer 2
- 复杂 Agent 系统:三层都做
- **永远先做 Layer 1**——投入产出比最高

### 几个反直觉的核心观点

这套 Skill 里有几个跟主流 prompt engineering 思路不一样的判断,值得提前知道:

1. **AGENTS.md 写得越完整越好是错的**——给人看的内容会稀释 Agent 的决策信号,要狠心删掉所有"为人类可读性服务"的章节
2. **失败经验最优形态不是 trajectory log**——而是蒸馏过的 AVOID 短句,一句话讲清楚一个具体禁忌
3. **强模型和小模型的工作流设计完全不同**——同一套范式硬套到不同规模的模型上一定翻车
4. **大多数项目不需要三角色工作流**——80% 的场景双角色甚至单角色就够,过度工程是行业最常见的失败模式

## 来源与致谢

这套方法论是综合多个外部输入沉淀出来的,主要参考:

- **AGENTS.md 开源标准** — Linux 基金会下属 Agentic AI Foundation 维护的跨工具规范(https://agents.md/)
- **李宏毅老师 ML 2026 春季课程** — 「Harness Engineering」一讲,提出了 Prompt → Context → Harness 的三层演进观
- **DeepMind AutoHarness 论文**(arxiv 2603.03329, 2026.3) — 让模型自动合成代码 harness,小模型加 harness 打败大模型
- **EvoMap × 清华 Gene 论文**(arxiv 2604.15097, 2026.4) — 经验对象的形态(Gene vs Skill)决定模型表现,AVOID 短句的有效性来自这篇

如果你想深入了解背后的研究,这四份是关键起点。

## 反馈与贡献

这套 Skill 仍在迭代中。如果你在使用过程中发现:

- 某条 AVOID 实际上是错的,或者有更好的版本
- 某个反模式描述不够准确
- 某个章节在某种语境下的应用经验值得补充
- 任何 typo 或表达不清的地方

欢迎提 Issue 或 PR。开源协作本身就是这套方法论里"持续迭代"思想的具体实践。

## License

CC BY 4.0 — 可自由使用、修改、分发,请保留出处。

---

*"模型不是不够聪明,只是没有被人类好好引导。"——李宏毅*
