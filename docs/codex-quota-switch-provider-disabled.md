# Codex 额度用完后，切换第三方 API 仍无法发送怎么办？

常见现象是：Codex 官方额度已经用完，随后在配置工具中切换到另一个 API Provider，但输入框或发送按钮仍然不可用。此时不要连续换 Key 或反复点击发送，因为问题可能发生在客户端状态、配置层级或接口兼容性，而不一定是新 Provider 没有额度。

## 先判断是哪一层没有切换

| 现象 | 更可能的原因 | 先做什么 |
| --- | --- | --- |
| 发送按钮始终灰色 | 原会话仍保留官方账号限额状态 | 新建会话或完全重启客户端 |
| 重启后仍请求旧域名 | 新配置没有被当前进程读取 | 核对 `config.toml` 位置和 `model_provider` |
| 请求新域名但返回 401 | Key、环境变量或鉴权头错误 | 检查 Key 来源，不要继续改模型名 |
| 返回 404 / model not found | URL 路径或模型 ID 不匹配 | 从服务控制台复制当前模型名 |
| 普通问答成功、真实仓库任务中断 | Responses / SSE / 工具调用不完整 | 做连续三次真实任务验收 |

## 第一步：关闭旧会话状态

切换 Provider 后，先结束仍显示原限额提示的会话，再完全退出并重新启动 Codex。只关闭聊天窗口不一定会结束后台进程。

重新启动后新建一个会话，先做只读任务，例如：

```text
只读取当前仓库，告诉我入口文件、测试命令和两个主要模块；不要修改任何文件。
```

如果新会话仍无法发送，再检查配置，不要把旧会话的灰色按钮直接当作新线路故障。

## 第二步：确认用户级配置真的生效

Codex 用户级配置通常位于：

```text
~/.codex/config.toml
```

自定义 Provider 的结构示例：

```toml
model = "从服务控制台复制的当前可用模型名"
model_provider = "your_gateway"

[model_providers.your_gateway]
name = "Your Gateway"
base_url = "https://你的网关地址/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

重点检查：

- `model_provider` 与 `[model_providers.your_gateway]` 的名称完全一致；
- `base_url` 是否需要 `/v1`，不要凭经验重复添加；
- `wire_api` 是否为 Provider 实际支持的协议；
- `model` 使用控制台展示的真实模型 ID，而不是营销名称；
- 如果设置过 `CODEX_HOME`，配置文件可能不在默认目录；
- 不要依赖项目内 `.codex/config.toml` 覆盖认证和 Provider 设置。

## 第三步：检查环境变量是否仍是旧 Key

很多配置工具只改写 Provider 地址，不会替你清除旧终端或启动器中保存的环境变量。检查当前进程读取的是哪个变量名，但不要在截图、Issue 或群聊中展示 Key 的真实值。

如果修改了环境变量，应从同一个终端重新启动 Codex；已经运行的进程不会自动获得新值。

## 第四步：用状态码分流

- `401 / 403`：检查 Key、鉴权头和模型权限；
- `404 / model not found`：检查 `base_url`、API 路径和模型 ID；
- `429`：检查余额、并发和速率限制，避免重试风暴；
- `502 / 503`：固定同一任务，换时间或模型做对照；
- 固定时长后断流：检查 SSE 透传、代理缓冲和空闲超时。

能完成一句聊天不等于能运行 Coding Agent。Codex 的真实任务还涉及连续流式事件、工具调用、文件读写和多轮回传。

## 15 分钟验收新 Provider

新线路能发送后，不要立即扩大充值。固定同一个仓库任务连续跑 3 次：

1. 读取入口、测试命令和主要模块；
2. 修改两个相关文件并运行测试；
3. 记录完成情况、重连次数、耗时和实际扣费。

可以先使用免费的 [Codex API 兼容性自测](https://t6016884321-maker.github.io/vidai-config-generator/codex-api-self-test.html?utm_source=github&utm_medium=repository&utm_campaign=codex_quota_switch_20261014) 做分层检查。

如果需要增加一条候选线路进行小额对照，VidAI／胃袋AI当前不提供免费 API 试用；最低 ¥9.9 获得 $50 API 额度，通过[重度用户接入入口](https://t6016884321-maker.github.io/vidai-config-generator/recruit-users.html?utm_source=github&utm_medium=article&utm_campaign=codex_quota_switch_20261014)首充按当前规则额外获得 5% 奖励额度。请先连续测试 3 次，不满意就不要扩大充值。

## 利益披露

本文由 VidAI／胃袋AI团队维护，团队同时提供商业 API 服务。上述排查方法适用于任意兼容 Provider；价格、模型和线路状态可能变化，应以各服务商控制台及用户自己的实测记录为准。
