# Codex 配了第三方 API，为什么仍走默认线路？

你已经修改了 `.codex/config.toml`，Codex 却仍使用旧模型、旧端点或默认 Provider。很多时候不是 API 线路故障，而是**配置层级放错了**。

## 最容易踩中的原因

如果你把下面这些键写进仓库内的 `.codex/config.toml`，Codex 会出于安全原因忽略它们，并在启动时给出警告：

- `openai_base_url`
- `model_provider`
- `model_providers`
- 与 Provider 认证相关的配置

自定义 Provider 应写入用户级配置：

```text
~/.codex/config.toml
```

如果设置过 `CODEX_HOME`，实际位置是 `$CODEX_HOME/config.toml`。

## 一个最小可检查结构

```toml
model = "从服务商控制台复制的当前可用模型名"
model_provider = "your_gateway"

[model_providers.your_gateway]
name = "Your Gateway"
base_url = "https://你的网关地址/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

检查重点：

1. `model_provider` 与 `[model_providers.your_gateway]` 的 ID 完全一致；
2. `base_url` 是否包含服务商要求的路径；
3. `env_key` 填的是环境变量名称，不是真实 API Key；
4. 模型名以服务商控制台当前显示为准，不照抄旧教程；
5. 修改后重启 Codex，再进行测试。

## 项目配置到底能做什么？

Codex 会从项目根目录向当前工作目录查找 `.codex/config.toml`。更接近当前工作目录的配置优先级更高，但项目必须被标记为可信。

项目配置适合保存仓库相关的普通设置；它不能重定向凭据、修改 Provider 认证或覆盖上述 Provider 选择。团队希望统一第三方 API 时，应通过受控的用户级配置或组织网关方案部署，不要把密钥和认证入口提交到仓库。

## 用单次覆盖区分“配置问题”和“线路问题”

Codex 支持使用专用参数或 `--config` 对一次运行进行覆盖。排错时，可以先临时覆盖模型选择：

```bash
codex --model "当前可用模型名"
```

如果单次覆盖生效，而常规启动仍走旧配置，优先检查配置文件位置、层级和拼写；如果二者都失败，再检查模型名、鉴权、Responses API 与网关兼容性。

不要把真实 API Key 直接写进命令行，避免进入 shell 历史。

## 5 分钟排错顺序

1. 确认当前使用的是 `~/.codex/config.toml` 或 `$CODEX_HOME/config.toml`；
2. 从项目 `.codex/config.toml` 移除 Provider 和认证键；
3. 检查 TOML 表名、引号和顶层键位置；
4. 确认环境变量只检查“是否存在”，不要打印真实值；
5. 使用服务商控制台当前模型名做一次小任务；
6. 再用同一真实仓库任务连续测试 3 次，记录断流、工具调用、重试和扣费。

```bash
test -n "$OPENAI_API_KEY" && echo "OPENAI_API_KEY 已设置" || echo "OPENAI_API_KEY 未设置"
```

## 什么时候才应怀疑 API 服务？

- Responses API 无法建立或事件结构不完整；
- SSE 长任务频繁无错误断流；
- 工具调用结果返回后无法继续；
- 失败重试造成重复扣费或用量无法核对；
- 同一个小任务连续 3 次无法稳定完成。

可使用免费的 [Codex API 兼容性自测](https://t6016884321-maker.github.io/vidai-config-generator/codex-api-self-test.html?utm_source=github&utm_medium=repository&utm_campaign=provider_config_ignored_20261012) 先区分配置问题与线路问题。如果需要加入一个候选线路对照，可在充分披露的前提下小额测试 [VidAI／胃袋 API](https://api.david-ai.net/?utm_source=github&utm_medium=repository&utm_campaign=provider_config_ignored_20261012)，不要未经验证直接大额充值。

## 官方依据

- [OpenAI 官方 Codex Advanced Configuration](https://learn.chatgpt.com/docs/config-file/config-advanced)

本文由 VidAI／胃袋AI团队维护，团队同时提供商业 API 服务。配置方法可用于排查任意兼容 Provider；模型、价格和线路状态以各服务商实时信息为准。
