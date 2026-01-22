# kiro-rs-oc

> 🔱 **OpenCode 特化版本** - 基于 [hank9999/kiro.rs](https://github.com/hank9999/kiro.rs) 的 Fork，专为 OpenCode 用户优化

[![GitHub release](https://img.shields.io/github/v/release/Kayphoon/kiro-rs-oc)](https://github.com/Kayphoon/kiro-rs-oc/releases)
[![Docker Image](https://img.shields.io/badge/docker-ghcr.io%2Fkayphoon%2Fkiro--rs--oc-blue)](https://ghcr.io/kayphoon/kiro-rs-oc)

一个用 Rust 编写的 Anthropic Claude API 兼容代理服务，将 Anthropic API 请求转换为 Kiro API 请求。

## 🆕 Fork 版本特性

相比上游版本，本 Fork 增加了以下优化：

- **🐳 Docker 一键部署** - 提供 docker-compose.yml，直接拉取运行
- **📦 GHCR 镜像** - 自动构建多架构镜像 (amd64/arm64)，无需本地编译
- **🔧 Token 统计优化** - 引入缓冲层修复首包 input_tokens 不准确问题
- **⚡ OpenCode 兼容** - 针对 OpenCode 使用场景测试和优化

## 🚀 快速开始（Docker 方式）

```bash
# 1. 下载 docker-compose.yml 和配置示例
curl -O https://raw.githubusercontent.com/Kayphoon/kiro-rs-oc/master/docker-compose.yml
curl -O https://raw.githubusercontent.com/Kayphoon/kiro-rs-oc/master/config.example.json

# 2. 创建配置文件
cp config.example.json config.docker.json
# 编辑 config.docker.json 填入你的配置

# 3. 启动服务
docker-compose up -d

# 4. 查看日志
docker-compose logs -f
```

## 免责声明
本项目仅供研究使用, Use at your own risk, 使用本项目所导致的任何后果由使用人承担, 与本项目无关。
本项目与 AWS/KIRO/Anthropic/Claude 等官方无关, 本项目不代表官方立场。

## 注意！
因 tls 库从 native-tls 切换至 rustls, 你可能需要专门安装证书后才能配置 HTTP PROXY

## 功能特性

- **Anthropic API 兼容**: 完整支持 Anthropic Claude API 格式
- **流式响应**: 支持 SSE (Server-Sent Events) 流式输出
- **Token 自动刷新**: 自动管理和刷新 OAuth Token
- **多凭据支持**: 支持配置多个凭据，按优先级自动故障转移
- **智能重试**: 单凭据最多重试 3 次，单请求最多重试 9 次
- **凭据回写**: 多凭据格式下自动回写刷新后的 Token
- **Thinking 模式**: 支持 Claude 的 extended thinking 功能
- **工具调用**: 完整支持 function calling / tool use
- **多模型支持**: 支持 Sonnet、Opus、Haiku 系列模型

## 支持的 API 端点

| 端点 | 方法 | 描述          |
|------|------|-------------|
| `/v1/models` | GET | 获取可用模型列表    |
| `/v1/messages` | POST | 创建消息（对话）    |
| `/v1/messages/count_tokens` | POST | 估算 Token 数量 |

## 安装方式

### 方式一：Docker（推荐）

```bash
# 直接使用 GHCR 镜像
docker pull ghcr.io/kayphoon/kiro-rs-oc:latest

# 或使用 docker-compose
docker-compose up -d
```

### 方式二：下载预编译二进制

从 [Releases](https://github.com/Kayphoon/kiro-rs-oc/releases) 下载对应平台的二进制文件：

| 平台 | 架构 | 文件名 |
|------|------|--------|
| macOS | ARM64 (M1/M2/M3) | `kiro-rs-oc-*-macOS-arm64` |
| macOS | Intel x64 | `kiro-rs-oc-*-macOS-x64` |
| Windows | x64 | `kiro-rs-oc-*-Windows-x64.exe` |
| Linux | x64 | `kiro-rs-oc-*-Linux-x64` |
| Linux | ARM64 | `kiro-rs-oc-*-Linux-arm64` |

### 方式三：从源码编译

> **前置步骤**：编译前需要先构建前端 Admin UI：
> ```bash
> cd admin-ui && npm install && npm run build
> ```

```bash
cargo build --release
./target/release/kiro-rs-oc -c config.json
```

## 配置文件

创建 `config.json` 配置文件：

```json
{
   "host": "127.0.0.1",
   "port": 8990,
   "apiKey": "sk-kiro-rs-qazWSXedcRFV123456",
   "region": "us-east-1"
}
```

详细配置说明请参考上游项目文档。

## 凭证文件

创建 `credentials.json` 凭证文件（从 Kiro IDE 获取）：

```json
{
   "refreshToken": "XXXXXXXXXXXXXXXX",
   "expiresAt": "2025-12-31T02:32:45.144Z",
   "authMethod": "social"
}
```

## 与上游的区别

| 特性 | 上游 kiro.rs | 本 Fork (kiro-rs-oc) |
|------|-------------|---------------------|
| Docker 镜像 | 需自行构建 | ✅ GHCR 预构建镜像 |
| docker-compose | 无 | ✅ 开箱即用 |
| Token 统计 | 可能不准确 | ✅ 缓冲层优化 |
| 配置挂载 | - | ✅ 支持外部配置文件 |

## License

MIT

## 致谢

本项目基于以下项目：
- [hank9999/kiro.rs](https://github.com/hank9999/kiro.rs) - 上游项目
- [kiro2api](https://github.com/caidaoli/kiro2api)
- [proxycast](https://github.com/aiclientproxy/proxycast)

感谢所有贡献者的努力！
