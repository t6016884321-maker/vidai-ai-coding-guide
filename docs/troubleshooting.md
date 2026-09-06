# 常见问题排查

## 401 / API_KEY_REQUIRED

含义：服务端没有收到有效密钥。

检查顺序：

1. 确认 API Key 没有多余空格、中文引号或换行。
2. 确认环境变量是在启动客户端的同一个终端中设置的。
3. 关闭并重新打开终端后，需要重新导出临时环境变量。
4. 确认密钥没有被撤销、过期或余额不足。

只检查是否存在，不要打印真实值：

```bash
test -n "$OPENAI_API_KEY" && echo "OPENAI_API_KEY 已设置" || echo "OPENAI_API_KEY 未设置"
```

```bash
test -n "$ANTHROPIC_AUTH_TOKEN" && echo "ANTHROPIC_AUTH_TOKEN 已设置" || echo "ANTHROPIC_AUTH_TOKEN 未设置"
```

## 模型不存在 / model not found

1. 回到胃袋AI控制台；
2. 复制当前标记为 Codex 或 Claude Code 可用的模型名；
3. 不要使用截图或旧文章中的模型名；
4. 重启客户端再测试。

## Codex 没有使用自定义 Provider

检查：

- 文件是否确实是 `~/.codex/config.toml`；
- `model_provider = "vidai"` 是否位于顶层；
- `[model_providers.vidai]` 拼写是否一致；
- Provider 是否误写到了项目级 `.codex/config.toml`；
- TOML 是否存在未闭合引号。

## Claude Code 仍然连接默认端点

检查当前终端：

```bash
echo "$ANTHROPIC_BASE_URL"
```

预期输出：

```text
https://api.david-ai.net
```

Windows PowerShell：

```powershell
$env:ANTHROPIC_BASE_URL
```

## 请求超时或连接失败

- 先检查胃袋AI官网和公告频道；
- 切换稳定网络后重试一次；
- 不要连续高频重试同一请求；
- 记录发生时间、客户端版本、错误码与请求类型后反馈；
- 反馈日志前删除 API Key、仓库私密信息和个人路径。

## 如何安全求助

可以提供：

- Codex / Claude Code 版本；
- 操作系统；
- 错误码与经过脱敏的错误文本；
- 配置键名，但把真实值替换为 `***`；
- 问题发生时间。

不要提供：

- 完整 API Key；
- 包含密钥的终端截图；
- 私有仓库源代码；
- 个人邮箱或账单信息。
