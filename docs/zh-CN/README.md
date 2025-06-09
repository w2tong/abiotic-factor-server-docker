# Palworld Dedicated Server Docker

## ⚠️These docs will be deprecated and removed on March 31 2024⚠️

Please use the official docs: [https://palworld-server-docker.loef.dev/](https://palworld-server-docker.loef.dev/zh/)

[![Release](https://img.shields.io/github/v/release/thijsvanloef/palworld-server-docker)](https://github.com/thijsvanloef/palworld-server-docker/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/thijsvanloef/palworld-server-docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Docker Stars](https://img.shields.io/docker/stars/thijsvanloef/palworld-server-docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Image Size](https://img.shields.io/docker/image-size/thijsvanloef/palworld-server-docker/latest)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker/tags)
[![Discord](https://img.shields.io/discord/1200397673329594459?logo=discord&label=Discord&link=https%3A%2F%2Fdiscord.gg%2FUxBxStPAAE)](https://discord.com/invite/UxBxStPAAE)
[![Static Badge](https://img.shields.io/badge/README-0.16.0-blue?link=https%3A%2F%2Fgithub.com%2Fthijsvanloef%2Fpalworld-server-docker%2Fblob%2Fmain%2FREADME.md)](https://github.com/thijsvanloef/palworld-server-docker?tab=readme-ov-file#palworld-dedicated-server-docker)
[![Docker Hub](https://img.shields.io/badge/Docker_Hub-palworld-blue?logo=docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/palworld)](https://artifacthub.io/packages/search?repo=palworld)

> [!CAUTION]
> The docs have been moved
> to: [https://palworld-server-docker.loef.dev/zh/](https://palworld-server-docker.loef.dev/zh/)

加入我们的 [Discord](https://discord.gg/UxBxStPAAE)

[English](/README.md) | [한국어](/docs/kr/README.md) | [简体中文](/docs/zh-CN/README.md)

> [!TIP]
> 不知道从何开始？ [看看这里吧！](https://tice.tips/containerization/palworld-server-docker/)

这是一个 [Docker](https://docs.docker.com/engine/install/) 容器，可帮助您创建自己的
[幻兽帕鲁](https://store.steampowered.com/app/1623730/Palworld/) 专用服务器。

此 Docker 容器经过测试，可在以下操作系统上运作：

* Linux (Ubuntu/Debian)
* Windows 10,11
* MacOS (包括 Apple Silicon M1/M2/M3).

此容器也经过测试，可在 `x64` 和 `ARM64` 基于的 CPU 架构上运作。

> [!IMPORTANT]
> 目前，Xbox Game Pass/Xbox 主机玩家无法加入专用服务器。
>
> 他们只能通过邀请码加入，并且最多允许 4 人游玩。

## 赞助商

向以下赞助商致以衷心的感谢！

<p align="center"><!-- markdownlint-disable-line --><!-- markdownlint-disable-next-line -->
<!-- sponsors --><a href="https://github.com/ShoeBoom"><img src="https://github.com/ShoeBoom.png" width="50px" alt="ShoeBoom" /></a>&nbsp;&nbsp;<a href="https://github.com/doomhound188"><img src="https://github.com/doomhound188.png" width="50px" alt="doomhound188" /></a>&nbsp;&nbsp;<a href="https://github.com/AshishT112203"><img src="https://github.com/AshishT112203.png" width="50px" alt="AshishT112203" /></a>&nbsp;&nbsp;<a href="https://github.com/pabumake"><img src="https://github.com/pabumake.png" width="50px" alt="pabumake" /></a>&nbsp;&nbsp;<!-- sponsors -->
</p>

## Official Documentation

[![Documentation](https://github.com/thijsvanloef/palworld-server-docker/assets/58031337/b92d76d1-5efb-438d-9ffd-5385544a831b)](https://palworld-server-docker.loef.dev/zh/)

## 服务器配置需求

| 资源   | 最小   | 推荐              |
|------|------|-----------------|
| CPU  | 4 核  | 4+ 核以上          |
| 内存   | 16GB | 推荐 32GB 以上以稳定运行 |
| 存储空间 | 8GB  | 20GB            |

## 开始使用

注意，您需要配置 [环境变量](#环境变量)。

### Docker Compose

您可以直接使用 [docker-compose.yml](/docker-compose.yml) 来配置您的服务器：

```yml
services:
  palworld:
    image: thijsvanloef/palworld-server-docker:latest
    restart: unless-stopped
    container_name: palworld-server
    ports:
      - 8211:8211/udp
      - 27015:27015/udp
    environment:
      - PUID=1000
      - PGID=1000
      - PORT=8211 # 可选但推荐
      - PLAYERS=16 # 可选但推荐
      - SERVER_PASSWORD=worldofpals # 可选但推荐
      - MULTITHREADING=true
      - RCON_ENABLED=true
      - RCON_PORT=25575
      - TZ=Asia/Shanghai
      - ADMIN_PASSWORD=adminPasswordHere
      - COMMUNITY=false  # 如果您希望服务器显示在社区服务器页中，请启用此选项（注意配置SERVER_PASSWORD!）
      - SERVER_NAME=World of Pals
      - SERVER_DESCRIPTION=palworld-server-docker by Thijs van Loef
    volumes:
      - ./palworld:/palworld/
```

### Docker Run

将`<palworld-folder>`修改为您自己的路径。

```bash
docker run -d \
    --name palworld-server \
    -p 8211:8211/udp \
    -p 27015:27015/udp \
    -v ./<palworld-folder>:/palworld/ \
    -e PUID=1000 \
    -e PGID=1000 \
    -e PORT=8211 \
    -e PLAYERS=16 \
    -e MULTITHREADING=true \
    -e RCON_ENABLED=true \
    -e RCON_PORT=25575 \
    -e TZ=Asia/Shanghai \
    -e ADMIN_PASSWORD=adminPasswordHere \
    -e SERVER_PASSWORD=worldofpals \
    -e COMMUNITY=false \
    -e SERVER_NAME=World of Pals \
    -e SERVER_DESCRIPTION=palworld-server-docker by Thijs van Loef \
    --restart unless-stopped \
    --stop-timeout 30 \
    thijsvanloef/palworld-server-docker:latest
```

作为一个替代方案，你可以复制[.env.example](/.env.example)文件，并把文件重命名为 **.env** 。
根据您的需求，查看[环境变量](#环境变量) 部分并调整。调整你的docker启动命令如下：

```bash
docker run -d \
    --name palworld-server \
    -p 8211:8211/udp \
    -p 27015:27015/udp \
    -v ./<palworld-folder>:/palworld/ \
    --env-file .env \
    --restart unless-stopped \
    --stop-timeout 30 \
    thijsvanloef/palworld-server-docker:latest
```

### Kubernetes

将此容器部署到 Kubernetes 的所有文件都位于[此文件夹中](/k8s/)。

请按照 [此处](/k8s/readme.md) 进行部署。

#### 使用 helm 部署

请按照 [此处](/charts/palworld/README.md) 进行部署。

### 环境变量

您可以使用以下值来修改服务器设置。

强烈建议您在启动服务器之前设置以下变量：

* PLAYERS
* PORT
* PUID
* PGID

| 变量                               | 信息                                               | 默认值                        | 允许值                                                                           |
|----------------------------------|--------------------------------------------------|----------------------------|-------------------------------------------------------------------------------|
| TZ                               | 备份服务器时所使用的时间戳。                                   | UTC                        | 参见 [时区列表](https://zh.wikipedia.org/wiki/%E6%97%B6%E5%8C%BA%E5%88%97%E8%A1%A8) |
| PLAYERS*                         | 可同加入服务器的最大玩家数。                                   | 16                         | 1-32                                                                          |
| PORT*                            | 服务器将开放的 UDP 端口。                                  | 8211                       | 1024-65535                                                                    |
| PUID*                            | 服务器运行时的用户的 UID。                                  | 1000                       | !0                                                                            |
| PGID*                            | 服务器运行时的组的 GID。                                   | 1000                       | !0                                                                            |
| MULTITHREADING**                 | 提高多线程 CPU 环境下的性能。它最多对 4 个线程有效，分配超过这个数量的线程没有太大意义。 | false                      | true/false                                                                    |
| COMMUNITY                        | 服务器是否显示在社区服务器页中（建议设置SERVER_PASSWORD）。            | false                      | true/false                                                                    |
| PUBLIC_IP                        | 您可以手动指定服务器 IP 地址。若未指定，将自动检测。                     |                            | x.x.x.x                                                                       |
| PUBLIC_PORT                      | 您可以手动指定服务器端口。若未指定，将自动检测。                         |                            | 1024-65535                                                                    |
| SERVER_NAME                      | 服务器名称。                                           |                            | "string"                                                                      |
| SERVER_PASSWORD                  | 为服务器设置密码。                                        |                            | "string"                                                                      |
| ADMIN_PASSWORD                   | 为服务器设置管理员密码。                                     |                            | "string"                                                                      |
| UPDATE_ON_BOOT**                 | 在启动 Docker 容器时更新/安装服务器（需要在第一次运行时启用）。             | true                       | true/false                                                                    |
| RCON_ENABLED***                  | 为服务器启用 RCON。                                     | true                       | true/false                                                                    |
| RCON_PORT                        | RCON 连接端口。                                       | 25575                      | 1024-65535                                                                    |
| QUERY_PORT                       | 用于与 Steam 服务器通信的查询端口。                            | 27015                      | 1024-65535                                                                    |
| BACKUP_CRON_EXPRESSION           | 自动备份的频率。                                         | 0 0 \* \* \*               | 需要一个Cron表达式 - 参见 [使用 Cron 执行自动备份](#使用-cron-执行自动备份)。                           |
| BACKUP_ENABLED                   | 启用自动备份。                                          | true                       | true/false                                                                    |
| DELETE_OLD_BACKUPS               | 在一定天数后删除备份。                                      | false                      | true/false                                                                    |
| OLD_BACKUP_DAYS                  | 保留备份的天数。                                         | 30                         | 任何正整数                                                                         |
| AUTO_UPDATE_CRON_EXPRESSION      | 自动更新的频率。                                         | 0 \* \* \* \*              | 需要一个Cron表达式 - 参见 [使用 Cron 执行自动更新](#使用-cron-执行自动更新)。                           |
| AUTO_UPDATE_ENABLED              | 启用自动更新。                                          | false                      | true/false                                                                    |
| AUTO_UPDATE_WARN_MINUTES         | 在通知玩家后等待多长时间更新服务器。                               | 30                         | !0                                                                            |
| AUTO_REBOOT_CRON_EXPRESSION      | 设置自动重启的频率。                                       | 0 0 \* \* \*               | 需要一个Cron表达式 - 参见 [使用 Cron 执行自动重启](#使用-cron-执行自动重启)。                           |
| AUTO_REBOOT_ENABLED              | 启用自动重启                                           | false                      | true/false                                                                    |
| AUTO_REBOOT_WARN_MINUTES         | 在通知玩家后等待多长时间重启服务器。                               | 5                          | !0                                                                            |
| DISCORD_WEBHOOK_URL              | Discord 服务器上创建 Webhook 后的 Discord Webhook URL    |                            | `https://discord.com/api/webhooks/<webhook_id>`                               |
| DISCORD_CONNECT_TIMEOUT          | Discord 命令初始连接超时                                 | 30                         | !0                                                                            |
| DISCORD_MAX_TIMEOUT              | Discord 超时时间                                     | 30                         | !0                                                                            |
| DISCORD_PRE_UPDATE_BOOT_MESSAGE  | 服务器开始更新时发送到 Discord 的消息                          | Server is updating...      | "string"                                                                      |
| DISCORD_POST_UPDATE_BOOT_MESSAGE | 服务器完成更新时发送到 Discord 的消息                          | Server update complete!    | "string"                                                                      |
| DISCORD_PRE_START_MESSAGE        | 服务器启动时发送到 Discord 的消息                            | Server is started!         | "string"                                                                      |
| DISCORD_PRE_SHUTDOWN_MESSAGE     | 服务器关闭时发送到 Discord 的消息                            | Server is shutting down... | "string"                                                                      |
| DISCORD_POST_SHUTDOWN_MESSAGE    | 服务器停止时发送到 Discord 的消息                            | Server is stopped!         | "string"                                                                      |

* 强烈建议设置。

** 启用该选项时，请确保您知道自己在做什么。

*** 是使用docker命令保存并关闭服务器的必要条件。

> [!IMPORTANT]
> 环境变量中使用的布尔值区分大小写，因为它们是在 shell 脚本中使用的。
>
> 必须准确使用 `true` 或 `false` 设置它们，选项才能生效。

### Game Ports

| 端口    | 信息            |
|-------|---------------|
| 8211  | 游戏端口 (UDP)    |
| 27015 | 查询端口 (UDP)    |
| 25575 | RCON 端口 (TCP) |

## 使用 RCON

RCON 已在此项目中默认开启。

使用 RCON CLI 非常简单：

```bash
docker exec -it palworld-server rcon-cli
```

这将打开一个 CLI，使用 RCON 使用服务器命令。

### 命令列表

| 命令                         | 信息             |
|----------------------------|----------------|
| Shutdown {秒} {信息}          | 服务器将在{秒}后关闭    |
| DoExit                     | 强制关闭服务器。       |
| Broadcast                  | 向服务器中所有玩家发送消息。 |
| KickPlayer {SteamID}       | 从服务器中踢出玩家。     |
| BanPlayer {SteamID}        | 从服务器中封禁玩家。     |
| TeleportToPlayer {SteamID} | 传送到目标玩家。       |
| TeleportToMe {SteamID}     | 将目标玩家传送到身边。    |
| ShowPlayers                | 显示所有已连接玩家信息。   |
| Info                       | 显示服务器信息。       |
| Save                       | 保存游戏。          |

请查看 [官方文档](https://tech.palworldgame.com/settings-and-operation/commands) 以获取所有命令。

## 创建备份

想要在游戏保存的时间点创建一个备份，使用以下命令：

```bash
docker exec palworld-server backup
```

执行后将在 `/palworld/backups/` 文件夹中生成备份。

若启用了 RCON，服务器将在备份前进行保存。

## 通过备份恢复数据

要从备份中恢复，请使用以下命令：

```bash
docker exec -it palworld-server restore
```

必须将 `RCON_ENABLED` 环境变量设置为 `true` 以使用此命令。

> [!IMPORTANT]
> 如果 Docker 重启策略不是设为 `always` 或 `unless-stopped`，那么服务器将会关闭，需要手动重新启动。
> 在 [开始使用](#开始使用) 中的示例 Docker run 命令和 Docker Compose 文件已经使用了所需的策略。

## 手动从备份中恢复数据

在 `/palworld/backups/` 中找到要恢复的备份并解压缩它。

删除位于 `/palworld/Pal/Saved/SaveGames/0/<old_hash_value>` 的旧保存数据文件夹。

将新解压缩的保存数据文件夹 `Saved/SaveGames/0/<new_hash_value>`
的内容复制到 `palworld/Pal/Saved/SaveGames/0/<new_hash_value>` 。

将 `palworld/Pal/Saved/Config/LinuxServer/GameUserSettings.ini` 中的 `DedicatedServerName` 替换为新文件夹名称。

```ini
DedicatedServerName=<new_hash_value>  # 替换为新的保存数据文件夹名称
```

重新启动游戏。（如果您正在使用 Docker Compose）

```bash
docker compose down && docker compose up -d
```

## 使用 Cron 执行自动备份

服务器将在每天午夜根据 TZ 设置的时区自动备份。

设置 `BACKUP_ENABLED` 启用或禁用自动备份（默认启用）。

`BACKUP_CRON_EXPRESSION` 是一个 cron 表达式，在 Cron 表达式中，您可以定义运行作业的间隔。

> [!TIP]
> 这个镜像使用 Supercronic 来执行 cron 任务。
>
> 查阅 [supercronic](https://github.com/aptible/supercronic#crontab-format)
> 或者
> [Crontab Generat](https://crontab-generator.org).

设置 `BACKUP_CRON_EXPRESSION` 以更改默认时程。

使用示例：如果 `BACKUP_CRON_EXPRESSION` 设置为 `0 2 * * *`，备份脚本将在每天凌晨 2:00 运行。

## 使用 Cron 执行自动更新

如果要在伺服器上使用自动更新功能， **必须** 将以下环境变数设置为 `true`：

* RCON_ENABLED
* UPDATE_ON_BOOT

> [!IMPORTANT]
> 如果 Docker 重启策略不是设为 `always` 或 `unless-stopped`，那么服务器将会关闭，需要手动重新启动。
> 在 [开始使用](#开始使用) 中的示例 Docker run 命令和 Docker Compose 文件已经使用了所需的策略。

设置 `AUTO_UPDATE_ENABLED` 以启用或禁用自动更新（默认为禁用）。

`AUTO_UPDATE_CRON_EXPRESSION` 是一个cron表达式，在Cron表达式中，需要定义了运行作业的间隔。

> [!TIP]
> 这个镜像使用 Supercronic 来执行 cron 任务。
> 查阅 [supercronic](https://github.com/aptible/supercronic#crontab-format)
> 或者
> [Crontab Generat](https://crontab-generator.org).

設置 `AUTO_UPDATE_CRON_EXPRESSION` 以更改默认时程。

## 使用 Cron 执行自动重启

为了能够使用该服务器的自动重启功能，需要启用 `RCON_ENABLED` 。

> [!IMPORTANT]
> 如果 Docker 重启策略不是设为 `always` 或 `unless-stopped`，那么服务器将会关闭，需要手动重新启动。
> 在 [开始使用](#开始使用) 中的示例 Docker run 命令和 Docker Compose 文件已经使用了所需的策略。

设置 `AUTO_REBOOT_ENABLED` 以启用或禁用自动备份（默认为禁用）。

`AUTO_REBOOT_CRON_EXPRESSION` 是一个cron表达式，在Cron表达式中，需要定义了运行作业的间隔。

> [!TIP]
> 该镜像使用 Supercronic 进行 cron 作业。
> 请参阅 [supercronic](https://github.com/aptible/supercronic#crontab-format)
> 或
> [Crontab Generator](https://crontab-generator.org).

设置 `AUTO_REBOOT_CRON_EXPRESSION` 以更改时程， 默认为每天午夜，根据设置的时区进行调整。

## 编辑服务器配置

### 使用环境变量

> [!IMPORTANT]
>
> 这些环境变量/设置可能会随着游戏的测试而改变。
>
> 查阅 [official webpage for the supported parameters](https://tech.palworldgame.com/optimize-game-balance) 。

| 环境变量                                      | 描述                                                                                       | 默认值                                                                                          | 可用值                                    |
|-------------------------------------------|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| DIFFICULTY                                | 游戏难度                                                                                     | None                                                                                         | `None`,`Normal`,`Difficult`            |
| DAYTIME_SPEEDRATE                         | 白天流逝速度 - 更小的值代表更快的白天                                                                     | 1.000000                                                                                     | Float                                  |
| NIGHTTIME_SPEEDRATE                       | 夜晚流逝速度 - 更小的值代表更快的夜晚                                                                     | 1.000000                                                                                     | Float                                  |
| EXP_RATE                                  | 经验值获取比率                                                                                  | 1.000000                                                                                     | Float                                  |
| PAL_CAPTURE_RATE                          | 帕鲁捕捉比率                                                                                   | 1.000000                                                                                     | Float                                  |
| PAL_SPAWN_NUM_RATE                        | 帕鲁刷新比率                                                                                   | 1.000000                                                                                     | Float                                  |
| PAL_DAMAGE_RATE_ATTACK                    | 多人游戏中来自帕鲁的伤害比率                                                                           | 1.000000                                                                                     | Float                                  |
| PAL_DAMAGE_RATE_DEFENSE                   | 多人游戏中对帕鲁的伤害比率                                                                            | 1.000000                                                                                     | Float                                  |
| PLAYER_DAMAGE_RATE_ATTACK                 | 多人游戏中来自玩家的伤害比率                                                                           | 1.000000                                                                                     | Float                                  |
| PLAYER_DAMAGE_RATE_DEFENSE                | 多人游戏中对玩家的伤害比率                                                                            | 1.000000                                                                                     | Float                                  |
| PLAYER_STOMACH_DECREASE_RATE              | 玩家饱食度流逝比率                                                                                | 1.000000                                                                                     | Float                                  |
| PLAYER_STAMINA_DECREASE_RATE              | 玩家体力减少比率                                                                                 | 1.000000                                                                                     | Float                                  |
| PLAYER_AUTO_HP_REGEN_RATE                 | 玩家生命值自动恢复比率                                                                              | 1.000000                                                                                     | Float                                  |
| PLAYER_AUTO_HP_REGEN_RATE_IN_SLEEP        | 玩家睡眠时生命值自动恢复比率                                                                           | 1.000000                                                                                     | Float                                  |
| PAL_STOMACH_DECREASE_RATE                 | 帕鲁饱食度流逝比率                                                                                | 1.000000                                                                                     | Float                                  |
| PAL_STAMINA_DECREASE_RATE                 | 帕鲁体力减少比率                                                                                 | 1.000000                                                                                     | Float                                  |
| PAL_AUTO_HP_REGEN_RATE                    | 帕鲁生命值自动恢复比率                                                                              | 1.000000                                                                                     | Float                                  |
| PAL_AUTO_HP_REGEN_RATE_IN_SLEEP           | 帕鲁生命值自动恢复比率（在终端盒中）                                                                       | 1.000000                                                                                     | Float                                  |
| BUILD_OBJECT_DAMAGE_RATE                  | 多人游戏中对建筑结构的伤害比率                                                                          | 1.000000                                                                                     | Float                                  |
| BUILD_OBJECT_DETERIORATION_DAMAGE_RATE    | 建筑结构的自然劣化比率                                                                              | 1.000000                                                                                     | Float                                  |
| COLLECTION_DROP_RATE                      | 多人游戏中道具掉落比率                                                                              | 1.000000                                                                                     | Float                                  |
| COLLECTION_OBJECT_HP_RATE                 | 多人游戏中对象生命比率                                                                              | 1.000000                                                                                     | Float                                  |
| COLLECTION_OBJECT_RESPAWN_SPEED_RATE      | 可获取对象的刷新速度 - 更小的值代表更快的刷新速度                                                               | 1.000000                                                                                     | Float                                  |
| ENEMY_DROP_ITEM_RATE                      | 多人游戏中敌人掉落道具的比率                                                                           | 1.000000                                                                                     | Float                                  |
| DEATH_PENALTY                             | 死亡惩罚</br>None: 没有死亡惩罚</br>Item: 更高几率掉落道具</br>ItemAndEquipment: 掉落所有道具</br>All: 掉落所有道具和帕鲁 | All                                                                                          | `None`,`Item`,`ItemAndEquipment`,`All` |
| ENABLE_PLAYER_TO_PLAYER_DAMAGE            | 允许 PVP 伤害                                                                                | False                                                                                        | Boolean                                |
| ENABLE_FRIENDLY_FIRE                      | 允许队友伤害                                                                                   | False                                                                                        | Boolean                                |
| ENABLE_INVADER_ENEMY                      | 启用营地入侵                                                                                   | True                                                                                         | Boolean                                |
| ACTIVE_UNKO                               | 启用 UNKO (?)                                                                              | False                                                                                        | Boolean                                |
| ENABLE_AIM_ASSIST_PAD                     | 启用控制器辅助瞄准                                                                                | True                                                                                         | Boolean                                |
| ENABLE_AIM_ASSIST_KEYBOARD                | 启用键盘辅助瞄准                                                                                 | False                                                                                        | Boolean                                |
| DROP_ITEM_MAX_NUM                         | 世界中最大掉落物品数量                                                                              | 3000                                                                                         | Integer                                |
| DROP_ITEM_MAX_NUM_UNKO                    | 世界中最大 UNKO 掉落数量                                                                          | 100                                                                                          | Integer                                |
| BASE_CAMP_MAX_NUM                         | 最大营地数量                                                                                   | 128                                                                                          | Integer                                |
| BASE_CAMP_WORKER_MAX_NUM                  | 营地最大工位数量                                                                                 | 15                                                                                           | Integer                                |
| DROP_ITEM_ALIVE_MAX_HOURS                 | 掉落物品移除时间                                                                                 | 1.000000                                                                                     | Float                                  |
| AUTO_RESET_GUILD_NO_ONLINE_PLAYERS        | 无玩家在线时自动重置公会                                                                             | False                                                                                        | Bool                                   |
| AUTO_RESET_GUILD_TIME_NO_ONLINE_PLAYERS   | 无玩家在线时自动重置公会的时间                                                                          | 72.000000                                                                                    | Float                                  |
| GUILD_PLAYER_MAX_NUM                      | 公会最大成员数量                                                                                 | 20                                                                                           | Integer                                |
| PAL_EGG_DEFAULT_HATCHING_TIME             | 孵化巨大型帕鲁蛋所需消耗时间（小时）                                                                       | 72.000000                                                                                    | Float                                  |
| WORK_SPEED_RATE                           | 多人游戏中的工作速度比率                                                                             | 1.000000                                                                                     | Float                                  |
| IS_MULTIPLAY                              | 启用多人游戏                                                                                   | False                                                                                        | Boolean                                |
| IS_PVP                                    | 启用 PVP                                                                                   | False                                                                                        | Boolean                                |
| CAN_PICKUP_OTHER_GUILD_DEATH_PENALTY_DROP | 允许玩家拾取其他公会玩家的掉落物品或死亡惩罚                                                                   | False                                                                                        | Boolean                                |
| ENABLE_NON_LOGIN_PENALTY                  | 启用无登录惩罚                                                                                  | True                                                                                         | Boolean                                |
| ENABLE_FAST_TRAVEL                        | 启用快速旅行                                                                                   | True                                                                                         | Boolean                                |
| IS_START_LOCATION_SELECT_BY_MAP           | 启用出生地选择                                                                                  | True                                                                                         | Boolean                                |
| EXIST_PLAYER_AFTER_LOGOUT                 | 玩家注销时是否删除玩家                                                                              | False                                                                                        | Boolean                                |
| ENABLE_DEFENSE_OTHER_GUILD_PLAYER         | 允许防御其他公会玩家                                                                               | False                                                                                        | Boolean                                |
| COOP_PLAYER_MAX_NUM                       | 工会中的最大玩家数量                                                                               | 4                                                                                            | Integer                                |
| REGION                                    | 地区                                                                                       |                                                                                              | String                                 |
| USEAUTH                                   | 使用授权验证                                                                                   | True                                                                                         | Boolean                                |
| BAN_LIST_URL                              | 使用禁用列表                                                                                   | [https://api.palworldgame.com/api/banlist.txt](https://api.palworldgame.com/api/banlist.txt) | string                                 |

## 修改服务器设置

服务器启动时，`PalWorldSettings.ini` 文件将在 `<mount_folder>/Pal/Saved/Config/LinuxServer` 文件夹中生成。

所有更改将在服务器下次启动时生效。

请注意，环境变量将始终覆盖对 `PalWorldSettings.ini` 所做的更改。

服务器配置文件参数相关说明，请在
[shockbyte](https://shockbyte.com/billing/knowledgebase/1189/How-to-Configure-your-Palworld-server.html) 中查看。

## 使用 Discord Webhooks

1. 在 Discord 的服务器设置中为你的 Discord 服务器生成一个 Webhook URL。

2. 使用 Discord Webhook URL 的范例，将唯一的令牌设置为环境变量，附在 URL
   的末尾，如下所示：`https://discord.com/api/webhooks/1234567890/abcde`

使用 Docker run 命令发送 Discord 消息：

```sh
-e DISCORD_WEBHOOK_URL="https://discord.com/api/webhooks/1234567890/abcde" \
-e DISCORD_PRE_UPDATE_BOOT_MESSAGE="Server is updating..." \
```

使用 Docker Compose 命令发送 Discord 消息：

```yaml
- DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/1234567890/abcde
- DISCORD_PRE_UPDATE_BOOT_MESSAGE=Server is updating...
```

## 锁定特定的游戏版本

> [!WARNING]
> 降级到更低的游戏版本是可能的，但目前不清楚它对现有存档会有什么影响。
>
>**请自行承担风险！**

如果 **TARGET_MANIFEST_ID** 环境变量被设置，服务器将锁定到特定的 manifest 版本。
manifest 对应于发布日期/更新版本。manifest 可以在 SteamCMD 或 [SteamDB](https://steamdb.info/depot/2857201/manifests/)
等网站上找到。

### 版本和 manifest 对照表

| 版本    | Manifest ID         |
|---------|---------------------|
| 0.1.3.0 | 1354752814336157338 |
| 0.1.4.0 | 4190579964382773830 |
| 0.1.4.1 | 6370735655629434989 |
| 0.1.5.0 | 3750364703337203431 |
| 0.1.5.1 | 2815085007637542021 |
| 0.2.0.6 | 1677469329840659324 |
| 0.2.1.0 | 8977386334474359538 |
| 0.2.2.0 | 3713294686782778004 |
| 0.2.3.0 | 5441332432956841998 |
| 0.2.4.0 | 3872500952532478729 |
| 0.3.1   | 4278745071359475598 |
| 0.3.2   | 7197559707261547198 |
| 0.3.3   | 8939748203712584968 |
| 0.3.4   | 2579525995905578621 |
| 0.3.5   | 6538676263384401448 |
| 0.3.6   | 7750307484972290423 |
| 0.3.7   | 2342038768084482228 |
| 0.3.8   | 8676441150170012909 |
| 0.3.9   | 7493245879597781625 |
| 0.3.10  | 752220234171168889  |

## 报告问题/功能请求

可通过 [此链接](https://github.com/thijsvanloef/palworld-server-docker/issues/new/choose) 报告问题/功能请求。

### 已知问题

已知问题已在 [Wiki](https://github.com/thijsvanloef/palworld-server-docker/wiki/Known-Issues) 中列出。
