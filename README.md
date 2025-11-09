# IoM (Internal of Malice)

## 项目简介

IoM 是由 ChainReactors 开发的进攻性基础设施框架，包含了C2, 代理， implant， SDK等组件， 计划构建完备的下一代进攻性基础设施。本仓库作为**导航项目**，整合了服务端、植入体、协议定义、插件系统和多语言 SDK 等核心组件。

**官方文档**: https://chainreactors.github.io/wiki/IoM/

## 项目结构

本项目通过 Git Submodules 管理以下核心组件：

```
project-IoM/
├── malice-network/          # C2 服务端和客户端
├── implant/
│   ├── malefic/            # 主植入体
│   ├── malefic-srid/       # SRDI 实现
│   └── malefic-3rd-template/ # 第三方模块模板
├── proto/                   # 协议定义
├── mals/
│   ├── mals/               # 插件框架
│   ├── mal-community/      # 社区插件
│   └── mal-intl/           # community版本内置插件
└── sdk/
    ├── IoM-go/             # Go SDK
    ├── IoM-python/         # Python SDK
    └── IoM-typescript/     # TypeScript SDK
```

## 核心组件

### 1. [malice-network](https://github.com/chainreactors/malice-network)
**C2 服务端和客户端基础设施**

- **语言**: Go 1.20+
- **功能**: gRPC 服务端、交互式客户端、监听器管理、会话控制
- **特性**: mTLS 认证、多监听器类型、Pipeline 流量路由、团队协作

### 2. [implant/malefic](https://github.com/chainreactors/malefic)
**主植入体 (Agent/Beacon)**

- **语言**: Rust
- **功能**: 模块化植入体、动态加载、进程注入、反射执行
- **特性**: 反沙箱/虚拟机/调试器、系统调用混淆、OLLVM 混淆、睡眠掩码

### 3. [implant/malefic-srid](https://github.com/chainreactors/malefic-srdi)
**Shellcode 反射 DLL 注入**

- **语言**: Rust
- **功能**: SRDI (Shellcode Reflective DLL Injection) 实现

### 4. [implant/malefic-3rd-template](https://github.com/chainreactors/malefic-3rd-template)
**第三方模块开发模板**

- **语言**: Rust
- **功能**: 扩展植入体能力的模块开发框架

### 5. [proto](https://github.com/chainreactors/proto)
**Protocol Buffers 协议定义**

- **语言**: Protobuf
- **功能**: gRPC 服务定义、客户端/植入体消息协议
- **包含**: 133+ gRPC 方法定义

### 6. [mals/mals](https://github.com/chainreactors/mals)
**插件系统核心框架**

- **语言**: Go + Lua
- **功能**: Lua 脚本引擎、插件加载器、命令扩展系统

### 7. [mals/mal-community](https://github.com/chainreactors/mal-community)
**社区贡献插件集合**

- **语言**: Go + Lua
- **功能**: 社区开发的扩展插件

### 8. [mals/mal-intl](https://github.com/chainreactors/mal-intl)
**国际化插件**

- **语言**: Go + Lua
- **功能**: 多语言支持和本地化

### 9. [sdk/IoM-go](https://github.com/chainreactors/IoM-go)
**Go 语言 SDK**

- **语言**: Go
- **功能**: gRPC 客户端库、会话管理、事件监听
- **用途**: Go 程序自动化操作 C2

### 10. [sdk/IoM-python](https://github.com/chainreactors/IoM-python)
**Python 语言 SDK**

- **语言**: Python 3.10+
- **功能**: 异步 gRPC 客户端、类型提示、动态方法转发
- **用途**: Python 脚本自动化操作 C2

### 11. [sdk/IoM-typescript](https://github.com/chainreactors/IoM-typescript)
**TypeScript 语言 SDK**

- **语言**: TypeScript
- **功能**: 类型安全的 gRPC 客户端
- **用途**: VSCode 扩展和 Web UI 开发

## 快速开始

### 克隆项目（包含所有子模块）

```bash
git clone --recursive https://github.com/chainreactors/malice-network.git project-IoM
cd project-IoM
```

### 更新所有子模块

```bash
git submodule update --init --recursive
```

### 拉取最新更改

```bash
git pull --recurse-submodules
git submodule update --remote --merge
```

## 技术栈概览

| 组件 | 语言 | 用途 |
|------|------|------|
| malice-network | Go | C2 服务端/客户端 |
| malefic | Rust | 植入体 |
| proto | Protobuf | 协议定义 |
| mals | Go + Lua | 插件系统 |
| SDK | Go/Python/TS | 自动化接口 |

## 架构设计

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   Operator  │◄───────►│    Server    │◄───────►│   Implant   │
│  (Client)   │  gRPC   │ (malice-net) │  TCP/   │  (malefic)  │
│             │  mTLS   │              │  HTTP   │             │
└─────────────┘         └──────────────┘         └─────────────┘
                              │
                              ├── Listeners
                              ├── Pipelines
                              ├── Database
                              └── Plugins (mals)
      ┌───────────────────────┴───────────────────────┐
      │                                               │
┌─────▼──────┐  ┌──────────────┐  ┌────────────────┐
│  IoM-go    │  │ IoM-python   │  │ IoM-typescript │
│  (SDK)     │  │  (SDK)       │  │    (SDK)       │
└────────────┘  └──────────────┘  └────────────────┘
```

## 安全声明

⚠️ **本项目仅供授权的安全测试、防御性安全研究、CTF 竞赛和教育用途。**

使用本工具需要明确的授权场景：
- 渗透测试合约
- CTF 竞赛
- 安全研究
- 防御性用例

禁止用于未经授权的攻击、破坏性行为或恶意活动。


## 相关链接

- **官方文档**: https://chainreactors.github.io/wiki/IoM/
- **开发团队**: ChainReactors

---

**⚡ Built by ChainReactors**
