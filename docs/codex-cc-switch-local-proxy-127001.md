# Codex 配置出现 127.0.0.1 正常吗？CC Switch 本地代理排错

先说结论：如果你在 CC Switch 中启动了“路由”或“聚合”，Codex 配置被写成 `http://127.0.0.1:15721` 通常是正常的。`127.0.0.1` 是本机回环地址，Codex 先把请求交给 CC Switch 的本地路由，再由它转发到当前供应商。

如果 CC Switch 已经显示“直连”，配置仍然指向 `127.0.0.1`；或者该端口根本没有进程监听，才需要继续排查。

## 先分清三种状态

| 当前状态 | 配置中的地址 | 含义 |
| --- | --- | --- |
| 路由 / 聚合已启动 | 通常是 `http://127.0.0.1:15721` | 正常，本地路由负责转发 |
| 已点击“回到直连” | 应写回直连供应商地址 | 若仍是本地地址，检查退出和配置写回 |
| 使用 OpenAI 官方登录 | 官方订阅请求不应依赖第三方本地路由 | 先回到直连，再按 Codex 自身登录流程操作 |

端口 `15721` 是 CC Switch 当前文档中的默认值；如果你改过端口，以应用实际设置为准。

## 127.0.0.1 不是远端 API 地址

`127.0.0.1` 只指向当前机器。路由模式下的请求链路是：

```text
Codex → 127.0.0.1:15721 → CC Switch → 当前选中的 API Provider
```

因此，只看 `config.toml` 里的本地地址，无法判断最后使用了哪家 Provider。应在 CC Switch 侧栏的“用量统计”请求日志里查看“请求模型 → 实际模型”、状态码和转发结果。

## 第一步：确认本地端口是否监听

macOS / Linux：

```bash
lsof -nP -iTCP:15721 -sTCP:LISTEN
```

Windows PowerShell：

```powershell
Get-NetTCPConnection -LocalPort 15721 -State Listen
```

- 路由已启动且有进程监听：本地入口基本正常，继续看请求日志；
- 路由已启动但没有监听：重启 CC Switch，再检查端口冲突或安全软件拦截；
- 已回到直连：没有本地监听并不异常，此时重点看配置是否已写回。

不要把真实 API Key、完整请求头或私有代码贴进 Issue、群聊和截图。

## 第二步：确认 Codex 读取的是哪份配置

OpenAI 官方配置参考说明，用户级配置位于：

```text
~/.codex/config.toml
```

如果设置过 `CODEX_HOME`，配置位置会随之改变。项目目录中的 `.codex/config.toml` 不能覆盖 `model_provider`、`model_providers`、`openai_base_url` 等机器本地 Provider 设置。

检查时不要只搜索当前项目，至少同时确认：

1. 实际的 `CODEX_HOME`；
2. 用户级 `config.toml`；
3. CC Switch 为 Codex 设置的配置目录；
4. 是否有另一个 Codex 进程仍读取旧配置。

## 第三步：回到直连后彻底重启 Codex

在 CC Switch 中点击“回到直连”，再检查配置是否恢复为直连供应商。Codex 可能仍保留启动时读取的模型和 Provider 状态：

- Codex CLI：结束仍在运行的 Codex 后台进程，再重新启动；
- Codex 桌面版：macOS 使用 `⌘Q` 完全退出，只关窗口不够；
- 编辑器插件：重新加载或重开编辑器窗口。

路由运行期间不建议手工把 `127.0.0.1` 改成远端地址，因为 CC Switch 可能再次接管并写回配置。要么继续使用路由并从日志排错，要么先明确回到直连。

## 第四步：特别检查 WSL2

CC Switch 文档提醒：Windows 上的 CC Switch 使用本地路由时，WSL2 默认 NAT 模式中的 `127.0.0.1` 指向 WSL 自己，无法直接连接 Windows 宿主机的本地路由。可改用 WSL mirrored 网络模式，或按 CC Switch 文档配置对应工具在 WSL 内的目录和网络。

典型现象是：Windows 端端口正常监听，但在 WSL 中运行 Codex 时连接被拒绝。

## 一张表定位故障

| 现象 | 更可能的原因 | 处理方向 |
| --- | --- | --- |
| 配置是 127.0.0.1，CC Switch 日志有成功请求 | 正常路由 | 无需修改配置 |
| 配置是 127.0.0.1，本地端口未监听 | 路由未启动或进程异常 | 重启 CC Switch、检查端口冲突 |
| 已回到直连，配置仍是 127.0.0.1 | 配置未写回或看错配置目录 | 核对 `CODEX_HOME` 和 CC Switch 配置目录 |
| Windows 正常，WSL 连接被拒绝 | WSL2 NAT 的回环隔离 | 使用 mirrored 网络或按官方文档配置 |
| 本地路由可连接，但返回 401 / 404 | 上游 Key、模型或路径错误 | 查看 CC Switch 请求日志，不要继续改本地端口 |
| 切换供应商后仍使用旧模型 | Codex 进程缓存了启动状态 | 完全重启 Codex |

## 最后做一次真实任务验收

端口通了不等于 Coding Agent 能稳定完成任务。固定同一个仓库任务连续运行 3 次，记录完成情况、重连次数、耗时与实际扣费，再决定是否扩大充值。

- 免费自测：[Codex API 兼容性自测](https://t6016884321-maker.github.io/vidai-config-generator/codex-api-self-test.html?utm_source=github&utm_medium=article&utm_campaign=codex_local_proxy_20261015&utm_content=self_test)
- 候选线路：[开放重度用户接入测试名额](https://t6016884321-maker.github.io/vidai-config-generator/recruit-users.html?utm_source=github&utm_medium=article&utm_campaign=codex_local_proxy_20261015&utm_content=recruit)

VidAI／胃袋AI目前不提供免费 API 试用；最低 ¥9.9 获得 $50 API 额度，通过邀请入口首充按当前规则额外获得 5% 奖励额度。请先做小额、连续、可记录的真实任务测试，不满意就不要扩大充值。

## 参考资料

- [CC Switch 中文 README：路由后为什么出现 127.0.0.1](https://github.com/farion1231/cc-switch/blob/main/README_ZH.md#开始路由后配置文件里的地址为什么变成了-127001)
- [OpenAI Codex Configuration Reference](https://developers.openai.com/codex/config-reference/)
- [相关社区问题：Discussion #8359](https://github.com/farion1231/cc-switch/discussions/8359)

## 利益披露

本文由 VidAI／胃袋AI团队维护，团队同时提供商业 API 服务。CC Switch 与 OpenAI 均未对 VidAI／胃袋AI作出推荐或背书；文中的软件行为以各自公开文档为准，线路表现、模型与价格以用户自己的实时测试为准。
