# Codex 自定义 API 出现 401：安全排查清单

`401` 通常表示请求已经到达接口，但服务端没有收到可接受的凭据。不要把 API Key 发到群聊、Issue 或截图里；下面的检查不需要输出密钥本身。

## 先确认变量存在

macOS / Linux：

```bash
if [ -n "$OPENAI_API_KEY" ]; then echo "OPENAI_API_KEY 已设置"; else echo "OPENAI_API_KEY 未设置"; fi
```

Windows PowerShell：

```powershell
if ($env:OPENAI_API_KEY) { "OPENAI_API_KEY 已设置" } else { "OPENAI_API_KEY 未设置" }
```

不要运行 `echo "$OPENAI_API_KEY"`，也不要把终端完整环境复制给别人。

## 按顺序检查

1. 在启动 Codex 的同一个终端窗口里设置环境变量。
2. 确认 `~/.codex/config.toml` 中自定义 Provider 的 `env_key` 与环境变量名称完全一致。
3. 确认密钥没有多余空格、换行或中文引号。
4. 在胃袋AI控制台确认该密钥仍有效且账户可用。
5. 关闭 Codex 后，从刚才确认变量存在的终端重新启动。

示例 Provider：

```toml
model = "从控制台复制的当前可用模型名"
model_provider = "vidai"

[model_providers.vidai]
name = "胃袋AI"
base_url = "https://api.david-ai.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

## 仍然失败时记录什么

- 日期、时间和时区；
- Codex 版本；
- 操作系统；
- 脱敏后的完整错误码；
- `env_key` 的名称，但不要提供值；
- 使用的模型名称与线路。

官方依据：[Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced)。自定义 Provider 可以通过 `env_key` 指定承载凭据的环境变量。

继续排查：[配置没有生效](codex-config-not-applied.md) · [模型不存在](codex-model-not-found.md)

