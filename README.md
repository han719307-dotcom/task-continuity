# Task Continuity

[English](#english) | [简体中文](#简体中文)

An instruction-only Agent Skill for keeping long-running work resumable across pauses, context compaction, and thread handoffs.

一个仅包含指令的 Agent Skill，用于让长时间、多阶段任务在暂停、上下文压缩或会话交接后仍可恢复。

---

## English

### Overview

Task Continuity maintains a compact, evidence-backed task card instead of attempting to preserve an entire conversation. It helps an agent decide when continuity management is useful, where one task should end, how to hand work off, and how to recover from verified workspace evidence.

This repository contains instructions and UI metadata only. There is no server to start, no dependency installation, and no build step.

### Features

- Maintains one concise `TASK_CARD.md` for active delivery state.
- Records completion criteria, verified evidence, next actions, constraints, and unresolved risks.
- Distinguishes continuation, independent delivery, and unrelated-task boundaries.
- Produces compact handoffs without copying the full conversation.
- Reconciles recovery claims against current files, diffs, tests, and artifacts.
- Explicitly excludes secrets, access tokens, private keys, and sensitive logs from task cards.
- Avoids unnecessary task cards for simple Q&A and single-turn edits.

### Repository structure

```text
task-continuity/
├── SKILL.md             # Skill metadata and complete workflow instructions
├── agents/
│   └── openai.yaml      # Display name, short description, and default prompt
├── .gitignore           # Excludes secrets, local state, dependencies, and outputs
└── README.md            # Bilingual project documentation
```

### Requirements

Use a host that supports the Agent Skills format, such as Codex CLI, the Codex IDE extension, or the Codex app. No programming language runtime or third-party package is required by this repository.

### Installation

Install the complete directory in one of Codex's skill locations:

- **User scope:** `$HOME/.agents/skills/task-continuity/`
- **Repository scope:** `<repository>/.agents/skills/task-continuity/`

For a user-scoped installation, clone directly into the skills directory.

macOS or Linux:

```bash
git clone https://github.com/han719307-dotcom/task-continuity.git "$HOME/.agents/skills/task-continuity"
```

Windows PowerShell:

```powershell
git clone https://github.com/han719307-dotcom/task-continuity.git "$HOME\.agents\skills\task-continuity"
```

Codex normally detects skill changes automatically. If the skill does not appear, restart Codex. For repository-scoped use, copy or clone this directory into `.agents/skills/task-continuity/` instead.

### Usage

There is no standalone startup command. After installation, invoke the skill explicitly in Codex:

```text
$task-continuity
```

Or include it in a request:

```text
Use $task-continuity to maintain a compact task card and verify where this long-running task should continue.
```

Codex can also select the skill automatically when a task matches the description in `SKILL.md`. Use `/skills` or the skill selector to confirm that it is available.

Typical scenarios include migrations, refactors, releases, research followed by implementation, long verification, and work that must survive a pause or handoff. It is not intended for simple Q&A, one-command checks, or edits completed in one turn unless continuity is explicitly requested.

### Validation

1. Confirm that `SKILL.md` contains `name` and `description` in its YAML frontmatter.
2. Confirm that `agents/openai.yaml` contains the intended display metadata.
3. Open the Codex skill selector and verify that **Task Continuity** appears.
4. Invoke `$task-continuity` on a multi-phase task and check that the resulting task card is concise, current, and supported by workspace evidence.

The project currently has no automated test suite because it contains no executable code.

### Security and local state

Do not store credentials, private keys, personal data, or complete sensitive logs in `TASK_CARD.md`, `SKILL.md`, or repository metadata. The root `.gitignore` excludes common secret files, local configuration, dependency directories, build outputs, and the local task card. Review the staged file list before every commit.

### Contributing

Keep the skill focused on continuity management. Preserve its concise task-card format, evidence-first recovery workflow, and instruction-only design. Update both language sections of this README when user-facing behavior or installation steps change.

### License

No open-source license has been selected yet. Until one is added, the default copyright rules apply.

---

## 简体中文

### 项目简介

Task Continuity 通过简短且有证据支撑的任务卡保存会影响后续行动的状态，而不是试图保留整段对话。它帮助智能体判断何时需要管理任务连续性、如何划分任务边界、如何交接，以及如何根据工作区证据恢复任务。

本仓库仅包含指令和界面元数据，不需要启动服务、安装依赖或执行构建。

### 主要功能

- 使用一份精简的 `TASK_CARD.md` 维护当前交付状态。
- 记录完成标准、已验证证据、下一步行动、约束和未解决风险。
- 区分同一交付的延续、独立交付和无关任务的边界。
- 生成交接摘要，避免复制整段对话。
- 恢复任务时，根据当前文件、差异、测试和产物核验既有结论。
- 明确禁止在任务卡中记录密钥、访问令牌、私钥和敏感日志。
- 避免为简单问答和单轮即可完成的小修改创建不必要的任务卡。

### 项目结构

```text
task-continuity/
├── SKILL.md             # Skill 元数据与完整工作流说明
├── agents/
│   └── openai.yaml      # 显示名称、简短说明和默认提示词
├── .gitignore           # 排除密钥、本地状态、依赖和构建产物
└── README.md            # 中英文双语项目文档
```

### 环境要求

需要支持 Agent Skills 格式的宿主，例如 Codex CLI、Codex IDE 扩展或 Codex 应用。本仓库自身不依赖任何编程语言运行时或第三方软件包。

### 安装方式

将完整目录安装到 Codex 的任一 Skills 目录：

- **用户级：** `$HOME/.agents/skills/task-continuity/`
- **仓库级：** `<repository>/.agents/skills/task-continuity/`

用户级安装可直接将仓库克隆到 Skills 目录。

macOS 或 Linux：

```bash
git clone https://github.com/han719307-dotcom/task-continuity.git "$HOME/.agents/skills/task-continuity"
```

Windows PowerShell：

```powershell
git clone https://github.com/han719307-dotcom/task-continuity.git "$HOME\.agents\skills\task-continuity"
```

Codex 通常会自动检测 Skill 变更；如果没有显示，请重启 Codex。仓库级使用则将此目录复制或克隆到 `.agents/skills/task-continuity/`。

### 使用与启动

本项目没有独立的启动命令。安装后，在 Codex 中显式调用：

```text
$task-continuity
```

也可以直接在请求中使用：

```text
Use $task-continuity to maintain a compact task card and verify where this long-running task should continue.
```

当任务符合 `SKILL.md` 中的描述时，Codex 也可以自动选择此 Skill。可通过 `/skills` 或 Skill 选择器确认它是否可用。

典型场景包括迁移、重构、发布、先研究后实施、长时间验证，以及需要跨暂停或交接继续的工作。除非用户明确要求管理连续性，否则它不适用于简单问答、单命令检查或单轮完成的修改。

### 验证方法

1. 确认 `SKILL.md` 的 YAML frontmatter 包含 `name` 和 `description`。
2. 确认 `agents/openai.yaml` 包含预期的显示元数据。
3. 打开 Codex Skill 选择器，确认能看到 **Task Continuity**。
4. 在一个多阶段任务中调用 `$task-continuity`，检查生成的任务卡是否精简、最新且有工作区证据支撑。

由于项目不包含可执行代码，目前没有自动化测试套件。

### 安全与本地状态

不要在 `TASK_CARD.md`、`SKILL.md` 或仓库元数据中保存凭据、私钥、个人数据或完整敏感日志。根目录 `.gitignore` 已排除常见密钥文件、本地配置、依赖目录、构建产物和本地任务卡。每次提交前仍应检查暂存文件清单。

### 贡献说明

请让 Skill 始终聚焦于任务连续性管理，保留精简任务卡、证据优先的恢复流程和纯指令设计。用户可见行为或安装步骤发生变化时，请同步更新本 README 的中英文部分。

### 许可证

尚未选择开源许可证。在添加许可证前，适用默认版权规则。

---

## References / 参考资料

- [OpenAI: Build skills](https://developers.openai.com/codex/build-skills)
- [Agent Skills specification](https://agentskills.io/specification)
