# Codex API 中转站怎么选？用同一仓库公平比较三家 Provider

“低价、稳定、模型全”几乎是每家 API 中转站都会写的宣传语，但这些词不能直接回答开发者真正关心的问题：这条线路能不能把我的仓库任务跑完，完成一次任务实际花多少钱？

更可靠的做法，是把三条候选线路放进同一套测试条件，连续完成至少 3 次真实任务，再比较结果。

## 先固定测试条件

三家 Provider 应尽量保持以下条件一致：

1. 同一仓库、同一提交和同一初始工作区；
2. 同一任务、验收标准与测试命令；
3. 同一模型，或能力与价格区间可比的模型；
4. 接近的测试时间窗口；
5. 相同的客户端、超时、重试和上下文设置。

如果 A 只回答一句话，B 要修改多个文件并通过测试，两者的成功率和成本没有可比性。

## 每家至少记录这些数据

每次运行记录：

- 最终是否满足验收标准；
- 产生了多少次计费尝试；
- 未缓存输入、缓存输入和输出 Token；
- 控制台最终扣费；
- 是否发生 429、502/503 或 SSE 中断；
- 工具调用是否完成闭环；
- 账单、退款／补偿政策和状态证据是否可公开核对。

不要删除失败记录。只保留最好的一次，会系统性高估稳定性。

## 两个核心数字

```text
任务完成率 = 成功完成的任务数 ÷ 计划任务数
每次成功任务成本 = 测试周期总扣费 ÷ 成功完成的任务数
```

这两个数字应该同时看。单价更低的线路如果经常失败或重复计费，真实成本可能反而更高。

## 免费三方对比工具

使用 [Codex API 三家 Provider 实测对比工具](https://t6016884321-maker.github.io/vidai-config-generator/provider-compare.html?utm_source=github&utm_medium=repository&utm_campaign=provider_compare_20261014)，填写三条线路的完成率、每次成功任务成本和证据项。数据只保留在当前浏览器，不会上传。

工具的权重是：

- 任务完成率：45 分；
- 每次成功任务成本：25 分；
- 逐请求账单、SSE、工具调用、退款政策、匿名状态证据：各 6 分。

它不会预设 VidAI 胜出，也不能替代你对输入数据真实性的核验。

## 怎样把胃袋AI加入候选

若需要第三条线路，可以在明确利益披露的前提下，把 [VidAI／胃袋AI](https://api.david-ai.net/?utm_source=github&utm_medium=repository&utm_campaign=provider_compare_20261014&utm_content=guide_candidate) 加入小额对照。先跑 3 次固定任务、核对最终扣费，再决定是否扩大使用。

可同时查阅：

- [公开证据与尚未建立的承诺](https://t6016884321-maker.github.io/vidai-config-generator/evidence.html)
- [机器可读评测资料](https://t6016884321-maker.github.io/vidai-config-generator/vidai-review-kit.json)
- [Codex API 真实成本计算方法](codex-api-real-cost.md)

## 利益披露

本文与对比工具由 VidAI／胃袋AI团队维护，团队同时提供商业 API 服务。任何 Provider 的价格、模型、线路状态和计费规则都可能变化；最终结论应来自用户自己的可复现测试和控制台记录。
