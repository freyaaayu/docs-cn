---
title: DM 集群软硬件环境需求
summary: 了解部署 DM 集群的软件和硬件要求。
aliases: ['/docs-cn/tidb-data-migration/dev/hardware-and-software-requirements/']
---

# DM 集群软硬件环境需求

DM 支持主流的 Linux 操作系统，具体版本要求见下表：

| Linux 操作系统平台       | 版本         |
| :----------------------- | :----------: |
| Red Hat Enterprise Linux | 7.3 及以上   |
| CentOS                   | 7.3 及以上   |
| Oracle Enterprise Linux  | 7.3 及以上   |
| Ubuntu LTS               | 16.04 及以上 |

DM 可以在 Intel 架构服务器环境及主流虚拟化环境中部署和运行。

## 服务器建议配置

DM 支持部署和运行在 Intel x86-64 架构的 64 位通用硬件服务器平台。对于开发，测试，及生产环境的服务器硬件配置（不包含操作系统本身的占用）有以下要求和建议：

### 开发及测试环境

| 组件 | CPU | 内存 | 本地存储 | 网络 | 实例数量（最低要求） |
| --- | --- | --- | --- | --- | --- |
| DM-master | 4 核+ | 8 GB+ | SAS，200 GB+ | 千兆网卡 | 1 |
| DM-worker | 8 核+ | 16 GB+ | SAS，200 GB+（大于迁移数据的大小） | 千兆网卡 | 上游 MySQL 实例的数量 |

> **注意：**
>
> - 在功能验证的测试环境中的 DM-master 和 DM-worker 可以部署在同一台服务器上。
> - 如进行性能相关的测试，避免采用低性能存储和网络硬件配置，防止对测试结果的正确性产生干扰。
> - 如果仅验证功能，可以单机部署一个 DM-master，DM-worker 部署的数量至少是上游 MySQL 实例的数量。为了保证高可用性，建议部署更多的 DM-worker。
> - DM-worker 在 `dump` 和 `load` 阶段需要存储全量数据，因此 DM-worker 的磁盘空间需要大于需要迁移数据的总量；如果迁移任务开启了 relay log，DM-worker 也需要一定的磁盘空间来存储上游的 binlog 数据。

### 生产环境

| 组件 | CPU | 内存 | 硬盘类型 | 网络 | 实例数量（最低要求） |
| --- | --- | --- | --- | --- | --- |
| DM-master | 4 核+ | 8 GB+ | SAS，200 GB+ | 千兆网卡 | 3 |
| DM-worker | 16 核+ | 32 GB+ | SSD，200 GB+（大于迁移数据的大小） | 万兆网卡 | 大于上游 MySQL 实例的数量 |
| 监控 | 8 核+ | 16 GB+ | SAS，200 GB+ | 千兆网卡 | 1 |

> **注意：**
>
> - 在生产环境中，不建议将 DM-master 和 DM-worker 部署和运行在同一个服务器上，以防 DM-worker 对磁盘的写入干扰 DM-master 高可用组件使用磁盘。
> - 在遇到性能问题时可参照[配置调优](/dm/dm-tune-configuration.md)尝试修改任务配置。调优效果不明显时，可以尝试升级服务器配置。
