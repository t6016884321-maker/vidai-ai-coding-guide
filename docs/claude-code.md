# Claude Code 接入胃袋AI

## 1. 前置准备

请先在胃袋AI控制台确认：

- Claude Code 当前可用模型；
- Claude Code 基础地址；
- 有效 API Key。

以下示例使用公开接口根地址。若控制台给出专用地址或额外变量，以控制台为准。

## 2. 当前终端配置

### macOS / Linux

```bash
export ANTHROPIC_BASE_URL="https://api.david-ai.net"
export ANTHROPIC_AUTH_TOKEN="你的胃袋AI_API_KEY"
```

### Windows PowerShell

```powershell
$env:ANTHROPIC_BASE_URL="https://api.david-ai.net"
$env:ANTHROPIC_AUTH_TOKEN="你的胃袋AI_API_KEY"
```

Anthropic 官方文档说明，`ANTHROPIC_BASE_URL` 用于把 Claude Code 请求路由到自定义 API 端点，`ANTHROPIC_AUTH_TOKEN` 可用于提供网关鉴权令牌。

## 3. 启动

```bash
cd 你的项目目录
claude
```

第一次建议发送：

```text
只读分析这个项目的目录结构、技术栈和测试方式，暂时不要修改文件。
```

## 4. 注意事项

- 不同网关可能要求不同的模型变量，请以胃袋AI控制台为准。
- `ANTHROPIC_BASE_URL` 只决定请求发往哪里，不自动决定模型名称。
- 不要同时保留相互冲突的旧代理环境变量。
- 不要把真实 API Key 写进项目代码或 `.env.example`。

遇到问题请查看 [排错手册](troubleshooting.md)。
