# Codex 提示 model not found：不要先猜模型名

`model not found` 不一定意味着网络不通。更常见的原因是模型名称过期、复制错误、当前线路不支持该模型，或 Codex 实际加载了另一个 Provider。

## 三分钟定位

1. 回到胃袋AI控制台，复制**当前**标记为 Codex 可用的完整模型名称。
2. 打开用户级 `~/.codex/config.toml`，检查顶层 `model` 与 `model_provider`。
3. 确认 Provider ID 一致：`model_provider = "vidai"` 对应 `[model_providers.vidai]`。
4. 保存配置，完全退出并重新启动 Codex。
5. 查看[实时线路状态](https://api.david-ai.net/monitor)，确认不是对应线路临时不可用。

```toml
model = "从胃袋AI控制台复制的当前可用模型名"
model_provider = "vidai"

[model_providers.vidai]
name = "胃袋AI"
base_url = "https://api.david-ai.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

## 容易踩的坑

- 从旧截图、聊天记录或旧教程复制模型名；
- 把展示名称当成接口模型 ID；
- 只改了 `model`，却仍然使用另一个 Provider；
- 把 Provider 配置写进项目 `.codex/config.toml`，期待它覆盖用户级鉴权配置；
- 把一次线路异常误判成模型永久下线。

OpenAI 官方文档说明：自定义 Provider 由顶层 `model_provider` 指向，并在 `[model_providers.<id>]` 下定义；模型与线路的实际可用情况仍以服务商实时控制台为准。

官方依据：[Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced)

继续排查：[401](codex-401.md) · [配置没有生效](codex-config-not-applied.md)

