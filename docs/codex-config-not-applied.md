# Codex 自定义 Provider 配置没有生效

症状通常是：修改了配置，但 Codex 仍走旧端点、旧模型，或者启动时看不到预期 Provider。

## 先检查文件位置

用户级配置默认位于：

```text
~/.codex/config.toml
```

Codex 的本地状态目录是 `CODEX_HOME`，默认值为 `~/.codex`。如果你自定义过 `CODEX_HOME`，配置文件位置也会随之变化。

## Provider 必须是用户级配置

```toml
model = "从胃袋AI控制台复制的当前可用模型名"
model_provider = "vidai"

[model_providers.vidai]
name = "胃袋AI"
base_url = "https://api.david-ai.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

不要依赖项目仓库里的 `.codex/config.toml` 来覆盖 `model_provider`、`model_providers` 或认证相关设置。OpenAI 官方文档说明，出于安全原因，这些键在项目级配置中会被忽略并产生启动警告。

## 检查 TOML 结构

- `model` 和 `model_provider` 位于顶层；
- `[model_providers.vidai]` 与 `model_provider = "vidai"` 拼写一致；
- 字符串引号成对；
- 不要把后续顶层键误写进 Provider 表格；
- 修改后重新启动 Codex。

## 只做一次临时验证

Codex 支持使用 CLI 覆盖单次运行的配置。排错时可使用官方文档列出的 `--model` 或 `--config` 机制验证模型选择，但不要把真实 API Key 放在命令历史里。

## 仍然不生效

记录 Codex 版本、配置文件的**脱敏结构**、启动警告与发生时间。把所有密钥值替换为 `***` 后再寻求帮助。

官方依据：[Codex Advanced Configuration](https://developers.openai.com/codex/config-advanced)

继续排查：[401](codex-401.md) · [模型不存在](codex-model-not-found.md)

