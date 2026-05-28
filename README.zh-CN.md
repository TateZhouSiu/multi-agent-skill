# Multi-Agent Skill

用于编写有边界、可审计 role brief 的 Agent Skill。

这个 skill 不等于必须启动子代理。它提供的是一种严格的任务 brief 格式，可用于探索、实现、审查、验证和交接。

## 它解决什么问题

把复杂模糊任务转成明确契约：

- 角色
- 目标
- 上下文
- 允许动作
- 写入边界
- 禁止动作
- 输出格式
- 停止条件

这样无论是真委派给子代理，还是自己按角色执行，都更容易控制范围和复查证据。

## 安装

把下面目录复制到兼容的 agent skills 目录：

```text
skills/multi-agent/
```

Codex 本地安装时，复制到本地 skills 目录后重启 agent。

## 运行环境

这个 skill 只有 Markdown 规则，没有操作系统级运行依赖。只要宿主 agent 支持本地 skills，macOS、Linux、WSL2 和 Windows 都可以使用。

模型建议：

- 简单 brief 编写，普通 coding model 就够。
- 安全审查、架构探索、长任务实现或多 agent 协作，建议使用更强的 reasoning model。
- 作者较重的工作流里，已验证画像是 Codex 风格 agent mode、GPT-5.5 级别 reasoning model，并在可用时使用 `high` 或 `xhigh` reasoning。
- 这个 skill 本身不要求多模态；只有被委派任务涉及图片、截图或视觉产物时，才需要多模态模型。

## Smoke Check

复制 `skills/multi-agent/` 到本地 skills 目录并重启宿主 agent 后，可以让它为一个只读探索任务起草 bounded brief。若宿主环境包含 Codex skill validator，该目录也应通过 `quick_validate.py`。

## 什么时候用

- 只读探索
- 有边界的实现
- 安全或正确性审查
- 修改后的验证
- 跨会话或跨 agent 交接

简单一步任务不需要使用。

## 与 AgentsMD Kit 的关系

AgentsMD Kit 是项目级规则和持久记忆。Multi-Agent Skill 是任务级 brief。两者独立开源和安装。
