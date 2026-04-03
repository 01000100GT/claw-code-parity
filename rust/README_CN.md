# 🦞 Claw Code — Rust 实现版

Claw Code CLI 代理框架的高性能 Rust 重写版本。专为速度、安全和原生工具执行而生。

export ANTHROPIC_AUTH_TOKEN="123"
export ANTHROPIC_API_KEY="321"
unset ANTHROPIC_API_KEY
unset ANTHROPIC_AUTH_TOKEN

export LLM_PROVIDER="openai"
export OPENAI_API_KEY="sk-867906122143c75489f93413cf9298c96f97222cdbe524070b55ee7561d6c42c"
export OPENAI_BASE_URL="<http://192.168.7.112:9081/v1>"
export OPENAI_MODEL_NAME="qwen3.5:35b"

## 快速开始

```bash
# 编译
cd rust/
cargo build --release

# 运行交互式 REPL (对话模式)
./target/release/claw

# 单次命令模式
./target/release/claw prompt "解释这个代码库"

# 指定模型运行
./target/release/claw --model gpt-4o prompt "修复 main.rs 中的 bug"
```

## 配置说明

支持 Anthropic、OpenAI 和 x.ai 等多种模型提供商。

### 1. Anthropic 配置 (默认)

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
# 可选：设置代理地址
export ANTHROPIC_BASE_URL="https://your-proxy.com"

cargo run --bin claw -- --model qwen3.5:35b

```

### 2. OpenAI 配置 (支持兼容接口)

项目内置了对 OpenAI 及其兼容服务的支持（如 SiliconFlow, DeepSeek 等）。

```bash
# 设置 API 密钥
export OPENAI_API_KEY="sk-..."
# 设置接口地址 (默认为 https://api.openai.com/v1)
export OPENAI_BASE_URL="https://api.openai.com/v1"

# 使用示例
./target/release/claw --model gpt-4o
```

### 3. x.ai 配置

```bash
export XAI_API_KEY="xai-..."
# 使用示例
./target/release/claw --model grok-beta
```

---

## 核心特性

| 特性 | 状态 |
|---------|--------|
| Anthropic API + 流式传输 | ✅ |
| OpenAI/x.ai 兼容接口 | ✅ |
| OAuth 登录/登出 | ✅ |
| 交互式 REPL (基于 rustyline) | ✅ |
| 工具系统 (bash, read, write, edit, grep, glob) | ✅ |
| Web 工具 (搜索, 抓取) | ✅ |
| 子代理编排 (Sub-agent) | ✅ |
| 任务追踪 (Todo Tracking) | ✅ |
| 项目记忆 (CLAUDE.md) | ✅ |
| 权限系统 (基于目录的读写控制) | ✅ |
| 扩展思维 (Thinking Blocks) | ✅ |
| 成本追踪与使用量显示 | ✅ |
| Git 集成 | ✅ |

## 模型别名 (Model Aliases)

为了方便使用，短名称会自动解析为最新的模型版本：

| 别名 | 解析为 | 提供商 |
|-------|------------|-------|
| `opus` | `claude-3-opus-20240229` | Anthropic |
| `sonnet` | `claude-3-5-sonnet-latest` | Anthropic |
| `haiku` | `claude-3-haiku-20240307` | Anthropic |
| `gpt-4o` | `gpt-4o` | OpenAI |

## 命令行参数

```
claw [OPTIONS] [COMMAND]

选项:
  --model MODEL                    设置模型 (别名或全名)
  --dangerously-skip-permissions   跳过所有权限检查
  --permission-mode MODE           设置权限模式: read-only, workspace-write, 或 danger-full-access
  --allowedTools TOOLS             限制启用的工具
  --output-format FORMAT           输出格式 (text 或 json)
```

## 工作区结构说明

- **crates/api** — HTTP 客户端，处理 SSE 流解析、认证（API Key + OAuth bearer）。支持 `ProviderKind` 扩展（Anthropic, OpenAI, Xai）。
- **crates/runtime** — 核心运行环境，管理对话循环、配置加载、权限策略及 MCP 客户端。
- **crates/tools** — 内置工具实现：Bash 脚本、文件操作、搜索、Web 访问等。
- **crates/mock-anthropic-service** — 确定性的本地 Mock 服务，用于一致性测试。

---

## 统计数据

- **约 20,000 行** Rust 代码
- **7 个核心模块 (Crates)**
- **二进制名称:** `claw`
- **默认模型:** `claude-3-5-sonnet-latest`
- **默认权限:** `danger-full-access` (开发环境)

## 许可证

详见存储库根目录。
