# Codex CLI 接入胃袋AI

## 1. 安装 Codex

macOS / Linux 官方安装命令：

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

验证安装：

```bash
codex --version
```

## 2. 获取必要信息

登录胃袋AI控制台，准备：

- 一个有效 API Key；
- 控制台当前展示的 Codex 可用模型名称。

不要根据旧教程猜测模型名称。模型列表可能变化。

## 3. 配置用户级 Provider

Codex 的 Provider 与鉴权配置必须写在用户级：

```text
~/.codex/config.toml
```

写入：

```toml
model = "从胃袋AI控制台复制的Codex可用模型名"
model_provider = "vidai"

[model_providers.vidai]
name = "胃袋AI"
base_url = "https://api.david-ai.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

不要把这段 Provider 配置放入项目的 `.codex/config.toml`。OpenAI 官方文档说明，项目级配置不能覆盖 Provider 与鉴权相关键。

## 4. 设置 API Key

### macOS / Linux：当前终端

```bash
export OPENAI_API_KEY="你的胃袋AI_API_KEY"
```

### macOS：长期保存到 zsh

```bash
echo 'export OPENAI_API_KEY="你的胃袋AI_API_KEY"' >> ~/.zshrc
source ~/.zshrc
```

### Linux：长期保存到 bash

```bash
echo 'export OPENAI_API_KEY="你的胃袋AI_API_KEY"' >> ~/.bashrc
source ~/.bashrc
```

### Windows PowerShell：当前窗口

```powershell
$env:OPENAI_API_KEY="你的胃袋AI_API_KEY"
```

### Windows PowerShell：保存到用户环境变量

```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "你的胃袋AI_API_KEY", "User")
```

保存后需要打开新的 PowerShell 窗口。

## 5. 启动并测试

```bash
cd 你的项目目录
codex
```

第一次建议使用只读任务：

```text
请先阅读当前项目结构，告诉我项目使用的技术栈，并给出三个可执行的优化建议。暂时不要修改文件。
```

## 6. 成功标准

- Codex 正常启动；
- 没有出现 `401`；
- 没有提示模型不存在；
- 可以读取当前目录并给出回答。

遇到问题请查看 [排错手册](troubleshooting.md)。
