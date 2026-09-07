# 胃袋AI × AI 编程工具

面向 Codex、Claude Code 与 Agent 高频开发用户的胃袋AI（VidAI）接入指南。

> 稳定接入 · 按量计费 · 性价比友好  
> 胃袋AI，量大管饱可劲造。

## 先用真实仓库验证

不要先相信“稳定”口号。按照 [Codex 三项真实仓库试跑](BENCHMARK.md) 记录成功率、重试次数、耗时、Token 和实际扣费，再决定是否继续使用。

- 免费可审查配置生成器：https://t6016884321-maker.github.io/vidai-config-generator/
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
