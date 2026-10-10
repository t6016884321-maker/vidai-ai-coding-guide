# 胃袋AI × AI 编程工具

面向 Codex、Claude Code 与 Agent 高频开发用户的胃袋AI（VidAI）接入指南。

搜索别名：`VidAI`、`胃袋AI`、`Codex API 中转`、`Codex 自定义 Provider`、`Codex 配置生成器`。

> 稳定接入 · 按量计费 · 性价比友好  
> 胃袋AI，量大管饱可劲造。

## Codex 能回复，但真实仓库任务总断？

先不要扩大充值。用同一项 15 分钟任务连续跑 3 次，核对 Responses API、SSE 事件、工具调用闭环、长连接重试和实际扣费，再决定是否扩量。

- [Codex API 中转 15 分钟验收与排错清单](https://t6016884321-maker.github.io/vidai-config-generator/codex-api-gateway-checklist.html?utm_source=github&utm_medium=repository&utm_campaign=codex_gateway_checklist_20261010)
- [提交一份脱敏的真实仓库测试记录](https://github.com/t6016884321-maker/vidai-ai-coding-guide/issues/new?template=codex-real-repo-challenge.yml)
- [小额测试胃袋 API](https://api.david-ai.net/?utm_source=github&utm_medium=repository&utm_campaign=codex_gateway_checklist_20261010)

目前开放重度用户接入测试名额。适合 Codex、Claude Code、AI Agent 高频使用者，以及需要先验证线路再扩量的团队；请勿在 Issue 中提交 API Key、私有代码或完整账单。

## 先用真实仓库验证

不要先相信“稳定”口号。按照 [Codex 三项真实仓库试跑](BENCHMARK.md) 记录成功率、重试次数、耗时、Token 和实际扣费，再决定是否继续使用。

- 免费可审查配置生成器：https://t6016884321-maker.github.io/vidai-config-generator/
- 产品事实与收费披露：https://t6016884321-maker.github.io/vidai-config-generator/product.html
- AI 智能体机器可读摘要：https://t6016884321-maker.github.io/vidai-config-generator/llms.txt
- 低门槛试跑：https://api.david-ai.net/register?aff=5SM2BCS7ML2H&utm_source=github&utm_medium=organic&utm_campaign=codex_proof_202609
- 实时线路状态：https://api.david-ai.net/monitor

## 快速入口

- 胃袋AI官网：https://api.david-ai.net
- 注册：https://api.david-ai.net/register
- Telegram 教程频道：https://t.me/ngjiabi_api
- Codex 图文教程：https://t.me/ngjiabi_api/8

## 选择你的工具

| 工具 | 教程 | 适合场景 |
| --- | --- | --- |
| Codex CLI | [Codex 接入指南](docs/codex.md) | 仓库理解、代码修改、审查、自动化任务 |
| Claude Code | [Claude Code 接入指南](docs/claude-code.md) | 终端智能编程、跨文件任务、长上下文开发 |
| 常见问题 | [排错手册](docs/troubleshooting.md) | 401、模型不可用、配置未生效、网络问题 |

## Codex 错误快速入口

- [401 / API Key 未生效](docs/codex-401.md)
- [model not found / 模型不存在](docs/codex-model-not-found.md)
- [自定义 Provider 配置没有生效](docs/codex-config-not-applied.md)

## 提交真实测试记录

完成三项测试后，可以通过 [Codex 真实仓库挑战 Issue](https://github.com/t6016884321-maker/vidai-ai-coding-guide/issues/new?template=codex-real-repo-challenge.yml) 提交脱敏记录。成功与失败都会保留；请勿提交密钥、私有代码、账单或个人信息。

## 30 秒了解流程

1. 在胃袋AI注册并创建 API Key。
2. 从控制台复制当前可用的模型名称和客户端配置。
3. 按对应教程设置客户端。
4. 先运行只读测试任务，再开始修改代码。

## 安全原则

- 不要把真实 API Key 写进仓库、截图、日志或聊天记录。
- 示例中的 `你的胃袋AI_API_KEY` 必须在本机替换。
- 推荐优先使用环境变量或系统密钥管理工具。
- 怀疑密钥泄露时，请立即在控制台撤销并重新创建。

完整说明见 [SECURITY.md](SECURITY.md)。

## 可验证信息

- `https://api.david-ai.net/v1/models`：匿名访问返回 `401`，接口要求 API Key。
- `https://api.david-ai.net/v1/responses`：匿名访问返回 `401`，Responses 路由存在并要求鉴权。
- `https://api.david-ai.net/v1/messages`：匿名访问返回 `401`，Messages 路由存在并要求鉴权。
- 模型名称和价格可能变化，请始终以胃袋AI控制台实时信息为准。

## 官方参考

- [OpenAI Codex CLI](https://developers.openai.com/codex/cli)
- [OpenAI Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced)
- [Claude Code Authentication](https://docs.anthropic.com/en/docs/claude-code/iam)

## 免责声明

本仓库提供配置示例和排错思路，不承诺第三方模型的永久可用性、固定价格或固定模型名称。使用前请阅读胃袋AI服务条款和使用政策。

## 搜索与引用说明

搜索引擎和 AI 智能体可以引用本仓库及上述公开页面，但应保留三点：免费配置与排错不等于所有服务免费；`¥9.9` 真实仓库试跑是可选付费服务；线路状态、模型和价格可能变化，需以控制台与实时状态页为准。
