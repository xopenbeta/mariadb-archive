# mariadb-archive

为 macOS 预编译的 MariaDB 二进制包，支持 Intel (x86_64) 和 Apple Silicon (arm64)。

## 触发构建

```bash
git tag v202603191350
git push
git push origin v202603191350
```

构建完成后，GitHub Release 页面会自动发布对应版本的两个平台二进制包。

## 支持版本

| 系列 | 最新版本 | 类型 |
|------|---------|------|
| 10.6 | 10.6.25 | Long-Term Support |
| 10.11 | 10.11.16 | Long-Term Support |
| 11.4 | 11.4.10 | Long-Term Support |
| 11.8 | 11.8.6 | Current Stable |
| 12.2 | 12.2.2 | Development |
| 12.3 | 12.3.1 | Development |

## 构建环境

| 架构 | Runner | 最低 macOS |
|------|--------|-----------|
| x86_64 (Intel) | `macos-13` | 12.0 Monterey |
| arm64 (Apple Silicon) | `macos-14` | 12.0 Monterey |

## 构建配置

- **SSL**: OpenSSL 3.x
- **默认字符集**: `utf8mb4` / `utf8mb4_general_ci`
- **禁用**: WSREP/Galera、RocksDB、Mroonga、jemalloc、单元测试
- **源码**: 从 [archive.mariadb.org](https://archive.mariadb.org) 下载

## 使用方法

从 [Releases](../../releases) 页面下载对应架构的压缩包：

```bash
# 解压
tar xzf mariadb-VERSION-macos-ARCH.tar.gz
cd mariadb-VERSION-macos-ARCH

# 初始化数据目录（首次使用）
./scripts/mariadb-install-db --datadir=$(pwd)/data --basedir=$(pwd)

# 启动服务
./bin/mariadbd --datadir=$(pwd)/data --basedir=$(pwd) \
  --socket=/tmp/mariadb.sock --port=3306 &

# 连接
./bin/mariadb --socket=/tmp/mariadb.sock

# 关闭服务
./bin/mariadb-admin --socket=/tmp/mariadb.sock shutdown
```

## 校验文件完整性

每个压缩包附带 SHA256 校验文件：

```bash
shasum -a 256 -c mariadb-VERSION-macos-ARCH.tar.gz.sha256
```
