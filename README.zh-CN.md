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

## 什么时候用

- 只读探索
- 有边界的实现
- 安全或正确性审查
- 修改后的验证
- 跨会话或跨 agent 交接

简单一步任务不需要使用。

## 与 AgentsMD Kit 的关系

AgentsMD Kit 是项目级规则和持久记忆。Multi-Agent Skill 是任务级 brief。两者独立开源和安装。
