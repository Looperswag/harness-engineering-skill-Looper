# AGENTS.md 模板

这是一个可直接填空的 AGENTS.md 模板。复制到你项目根目录的 `AGENTS.md`,按注释填写。

**为什么模板正文用英文?** AGENTS.md 是行业开源标准,各类 AI 编码工具都按英文链路解析它。最终产物保持英文能获得最稳定的工具兼容性,但填写过程中的指引和占位符是中文,方便中文用户填写。

**使用建议:**
- 不是所有 section 都必填,按你项目实际需要选
- 6 类必填内容用 ⭐ 标记
- 写完做一次"反向 review":每段问自己"这是给 AI 看的还是给人看的?"——给人看的删掉

---

## 模板正文(直接复制使用)

```markdown
# Project AGENTS.md

> Guidance for AI coding agents working on this project.
> For human-facing documentation, see README.md.

---

## ⭐ Project Context

<!--
  必填,但保持简短(50-100 字)。
  目标:让 Agent 30 秒内进入项目语境。
  写什么:项目类型、技术栈、用户、架构特点。
  不写什么:项目愿景、设计哲学、市场背景。
-->

This is a <项目类型> using <技术栈>. Users are <目标用户>.
Architecture pattern: <架构模式>.

Key technical context:
- <一个关键的代码库事实>
- <另一个关键事实>

---

## ⭐ Domain Vocabulary

<!--
  非常重要,但被 80% 的人写漏。
  这一节解决"模型用通用语义理解你的私有词汇"的问题。
  写什么:项目里跟通用含义偏差的术语、缩写、内部代号。
-->

- "<术语>" specifically refers to <项目内的特定含义>, NOT the generic <通用含义>.
- "<缩写>" stands for <完整形式>, not <其他常见展开>.
- Avoid using "<不推荐的词>" in code/comments — we use "<推荐的词>" instead.

---

## ⭐ Build & Test

<!--
  必填。Agent 会自己跑测试验证,不告诉它命令它会编造。
  下面的命令仅为格式示例,请替换成你项目实际使用的命令。
-->

```bash
# Setup
<安装依赖的命令>

# Build
<构建命令>

# Run all tests
<跑全部测试的命令>

# Run a single test
<跑单个测试的命令>

# Lint
<lint 命令>
```

After moving files or changing imports, ALWAYS run `<lint 命令>` to catch import errors.
Before any commit, run `<测试命令>` to ensure all tests pass.

---

## ⭐ Code Conventions

<!--
  关键原则:只写"反直觉的"或"项目特有的"约定。
  默认遵守语言/社区惯例的部分别写,Agent 默认就懂。
-->

- Indentation: <2-space / 4-space / tab>
- Naming: <文件命名 / 变量命名等约定>
- Imports: <import 顺序、分隔规则>
- Error handling: <Result 类型 / 异常 / 特定模式>

Project-specific conventions (these may surprise you):
- <反直觉的约定 1>
- <反直觉的约定 2>

---

## ⭐ Don't Do(AVOID list)

<!--
  整个 AGENTS.md 最关键的一节。
  写法:每条独立、自包含、针对一个具体错误。
  下面是常见的通用约束示例,根据项目实际情况增删。
-->

- Don't modify files in `<自动生成的目录>/` — they're regenerated on build
- Don't add new top-level dependencies without team discussion
- Don't refactor unrelated code in the same PR
- Don't hardcode secrets, API keys, or environment-specific values
- Don't silence linter warnings — fix them, or document why with a comment
- Don't commit `.env*` files
- Don't <项目特有的禁忌 1>
- Don't <项目特有的禁忌 2>

---

## ⭐ Workflow

<!--
  按你项目实际遇到的任务类型写。
  下面两类是大多数项目都需要的,可作为起点。
-->

### Workflow for Bug Fixes
1. First, run failing test to confirm reproduction
2. Read the relevant files (don't guess at structure)
3. Make minimal change addressing the root cause
4. Re-run the failing test, then run full test suite
5. Update CHANGELOG if user-facing

### Workflow for New Features
1. Check architecture decision document in `<文档路径>` first
2. Discuss approach in PR description before extensive coding
3. Add tests alongside the feature (not in a separate PR)
4. Update relevant docs

---

## PR Instructions(可选)

- Title format: `[<package>] <description>`
- Always run `<lint 和测试命令>` before commit
- Add tests for any logic change (even if not asked)

---

## Files to Read First(可选,大型 codebase 推荐)

For most tasks, read these files first to understand context:
- `<核心入口文件>`
- `<主要类型/Schema 定义>`
- `<路由/调度逻辑>`
```

---

## 填完后的 checklist

```
□ Project Context 是否在 100 字以内?(超过说明在写愿景,删)
□ Domain Vocabulary 是否真的列出了项目特有词汇?
□ Build & Test 命令是否实际可执行?(必须能复制粘贴就跑)
□ Code Conventions 是否只写了"反直觉"的部分?
□ AVOID 列表是否每条都"独立 + 具体 + 可验证"?
□ 全文是否在 100-400 行之间?
□ 全文是否删除了所有"为人类可读性服务"的内容?
   - "项目愿景""我们的使命""设计哲学""感谢贡献者" → 删
□ 全文是否避免了抽象原则?
   - "写干净的代码""保持高质量" → 改成可验证的具体规则
```

---

## 三个常见反模式

### 反模式 1:README 化

下面这种写法是错的——这些全是给人看的内容,Agent 不需要,会稀释信号:

```markdown
❌ # Project Vision
❌ Our mission is to revolutionize the way developers...
❌ ## Background
❌ In 2025, the team identified a gap in the market...
```

### 反模式 2:抽象原则化

模型对抽象原则响应度极低,要改成可验证的具体规则:

```markdown
❌ - Write clean, maintainable code
❌ - Follow best practices
❌ - Ensure good test coverage
```

改成:

```markdown
✅ - Run `pnpm lint` before any commit; fix all warnings
✅ - Test coverage must stay above 80%; verify with `pnpm test:coverage`
```

### 反模式 3:文档式堆砌

API 参考、详细类型定义这些应该在代码注释或独立文档里,AGENTS.md 不是 API 参考:

```markdown
❌ ## API Reference
❌ ### `getUser(id: string): User`
❌ Returns the user object for the given ID...
❌ [完整 API 文档复制粘贴]
```

---

## 跟 CLAUDE.md / .cursorrules 共存

**推荐方案:软链统一**

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md .cursorrules
```

维护一份,所有工具读到同一内容。

如果某个工具有独特配置(比如 Claude Code 的 MCP 服务器),共性写 AGENTS.md,工具特定的写在专属文件:

```
project-root/
├── AGENTS.md           # 共性规则,所有工具读
├── CLAUDE.md           # 软链到 AGENTS.md
└── .claude/
    └── mcp.json        # Claude Code 特有的 MCP 配置
```

---

## 一个完整示例

下面是一个紧凑且有效的 AGENTS.md 示例,基于一个虚构的 Python CLI 工具。这是一份填好后应该长什么样的"完成品",作为参考。

```markdown
# Project AGENTS.md

## Project Context

Python CLI tool for processing log files. Users are SREs and DevOps engineers.
Built on Click + Rich + Pydantic. Async I/O via asyncio.

## Domain Vocabulary

- "Pipeline" = a YAML-defined log processing flow, NOT a Python generator
- "Sink" = output destination (file, S3, stdout), NOT a database concept
- "Filter" = boolean predicate on log entries, NOT a UI filter

## Build & Test

```bash
poetry install
poetry run pytest                          # All tests
poetry run pytest tests/test_pipeline.py   # Single file
poetry run ruff check .                    # Lint
poetry run mypy src/                       # Type check
```

## Code Conventions

- 4-space indent, PEP 8 naming
- Type hints required for all public functions
- Use Pydantic models for any structured data, not dicts
- Async functions: prefix with `a_` (e.g., `a_fetch_logs`)

## Don't Do

- Don't add `requests` lib — we use `httpx` for sync/async unification
- Don't use `print()` — use `rich.print()` or the configured logger
- Don't hardcode timeouts — use `config.timeout_s`
- Don't add new CLI flags without updating the help text and docs

## Workflow for Bug Fixes

1. Reproduce with a failing test in `tests/`
2. Read the relevant pipeline definition in `src/pipelines/`
3. Make minimal fix
4. Verify: `poetry run pytest tests/test_<area>.py`
5. Run full suite: `poetry run pytest`
```

这个示例约 50 行,简洁但每一行都有用——这是好的 AGENTS.md 应该长的样子。
