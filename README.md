# ⚡ asscan - Cloudflare 反代 IP 极速扫描与优选工具

<p align="center">
  <img src="https://img.shields.io/badge/Language-Shell%20%2F%20Go-blue?style=flat-square" alt="Language">
  <img src="https://img.shields.io/badge/Platform-Linux%20(amd64)-green?style=flat-square" alt="Platform">
  <img src="https://img.shields.io/badge/License-MIT-orange?style=flat-square" alt="License">
</p>

`asscan` 是一个基于 **Masscan** 端口极速探测与 **Go 测速引擎** 的自动化脚本工具。支持按单个 ASN 自治系统或自定义 ASN 列表，自动提取全球 IP 范围、扫描指定端口开放情况，并针对可用 Cloudflare 反代节点进行精确的丢包率、延迟测速与地理位置标记。

---

## 🌟 核心特性

- 🚀 **极速并发探测**：底层基于 `masscan` 异步 SYN 扫描引擎，秒级遍历大段 ASN 网段。
- 🎯 **灵活扫描模式**：
  - **单个 AS 模式**：输入目标 ASN（如 `AS13335`、`AS45102`），自动拉取网段并探测。
  - **批量 AS 列表模式**：支持读取自定义列表（`as.txt`）执行无人值守自动化批量测速。
- 📊 **智能优选与测速**：内置高效测速模块，自动测试节点响应延迟、丢包率并关联所属数据中心（基于 `locations.json`）。
- 🛠️ **全自动环境适配**：自动识别 `Debian` / `Ubuntu` / `CentOS` / `Fedora` / `Alpine` 系统并自动补齐依赖（`libpcap`、`screen`、`curl`）。
- 🎛️ **多网卡智能管理**：单网卡自动绑定，多网卡模式支持交互式选择与持久化配置保存。

---

## 📋 运行环境要求

- **操作系统**：Linux（`Debian` / `Ubuntu` / `CentOS` / `Fedora` / `Alpine`）
- **架构**：`amd64` (x86_64)
- **运行权限**：`root` 用户（Masscan 底层收发裸包需要 Root 权限）

---

## 🚀 快速开始

### 1. 一键克隆与运行

```bash
# 克隆仓库
git clone https://github.com/starlgz/asscan.git
cd asscan

# 赋予执行权限并运行
chmod +x asscan.sh masscan iptest goscan
sudo bash asscan.sh
```

---

## 📖 菜单功能说明

运行脚本后将展示主交互菜单：

```text
==========================================
        asscan - CF 反代节点扫描工具
==========================================
1. 单个 AS 模式
2. 批量 AS 列表模式
3. 清空缓存数据
==========================================
```

### 🔹 1. 单个 AS 模式
- 输入你需要扫描的目标 ASN（例如输入 `45102`）；
- 指定扫描端口（支持 HTTP / HTTPS 常用端口，如 `80`, `443`, `8080`, `8443`, `2052`, `2053`, `2082`, `2083`, `2086`, `2087`, `2095`, `2096` 等）；
- 设置 Masscan 扫描速率（`rate`，默认根据机器带宽设定，如 `2000`~`10000`）；
- 脚本自动拉取 IP 范围并生成测速结果。

### 🔹 2. 批量 AS 列表模式
- 在项目根目录下创建 `as.txt`，每行填入一个 ASN 编号；
- 脚本将循环读取并自动完成全部 ASN 的扫描与综合节点测速。

### 🔹 3. 清空缓存数据
- 一键清理生成的临时 IP 列表、扫描抓包缓存及历史测速结果，释放磁盘空间。

---

## 📂 项目文件结构

```text
asscan/
├── asscan.sh        # 主运行与控制脚本（依赖检测、网卡配置、菜单调度）
├── masscan          # 预编译 Linux amd64 极速端口扫描二进制文件
├── iptest           # IP 节点可用性与延迟测速核心程序
├── goscan           # Go 编写的辅助分析与并发探测工具
├── locations.json   # 全球 Cloudflare 数据中心与地区代码映射字典
└── README.md        # 项目说明文档
```

---

## ⚠️ 注意事项与免责声明

1. **带宽与速率控制**：在公有云 VPS（如腾讯云、阿里云、AWS 等）上运行时，请合理设置 `masscan` 的扫描速率（Rate），避免因发包速率过高触发服务商滥用与风控告警。
2. **免责声明**：本项目仅供网络技术交流与合规网络测速使用，请勿用于任何未经授权的未成年目标网络探测或恶意网络攻击。

---

## 👤 作者与支持

- **Author**: [@starlgz](https://github.com/starlgz)
- **GitHub**: [https://github.com/starlgz/asscan](https://github.com/starlgz/asscan)
- 欢迎提交 Issue 与 Pull Request 共同完善！
