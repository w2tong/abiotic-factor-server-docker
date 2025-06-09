# Palworld 전용 서버 도커

## ⚠️These docs will be deprecated and removed on March 31 2024⚠️

Please use the official docs: [https://palworld-server-docker.loef.dev/](https://palworld-server-docker.loef.dev/ko/)

[![Release](https://img.shields.io/github/v/release/thijsvanloef/palworld-server-docker)](https://github.com/thijsvanloef/palworld-server-docker/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/thijsvanloef/palworld-server-docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Docker Stars](https://img.shields.io/docker/stars/thijsvanloef/palworld-server-docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Image Size](https://img.shields.io/docker/image-size/thijsvanloef/palworld-server-docker/latest)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker/tags)
[![Static Badge](https://img.shields.io/badge/readme-0.19.1-blue?link=https%3A%2F%2Fgithub.com%2Fthijsvanloef%2Fpalworld-server-docker%2Fblob%2Fmain%2FREADME.md)](https://github.com/thijsvanloef/palworld-server-docker?tab=readme-ov-file#palworld-dedicated-server-docker)
[![Discord](https://img.shields.io/discord/1200397673329594459?logo=discord&label=Discord&link=https%3A%2F%2Fdiscord.gg%2FUxBxStPAAE)](https://discord.com/invite/UxBxStPAAE)

[![Docker Hub](https://img.shields.io/badge/Docker_Hub-palworld-blue?logo=docker)](https://hub.docker.com/r/thijsvanloef/palworld-server-docker)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/palworld)](https://artifacthub.io/packages/search?repo=palworld)

> [!CAUTION]
> The docs have been moved to: [https://palworld-server-docker.loef.dev/ko/](https://palworld-server-docker.loef.dev/ko/)

[Discord에서 커뮤니티와 채팅하세요](https://discord.gg/UxBxStPAAE)

[English](/README.md) | [한국어](/docs/kr/README.md) | [简体中文](/docs/zh-CN/README.md)

> [!TIP]
> 어떻게 시작해야 할지 모르시나요? [제가 작성한 이 가이드](https://tice.tips/containerization/palworld-server-docker/)를 확인해 보세요

[Palworld](https://store.steampowered.com/app/1623730/Palworld/) 전용 서버 호스팅을 시작하는 데 도움이 되는 Docker 컨테이너입니다.

이 도커 컨테이너는 테스트되었으며 Linux(Ubuntu/Debian), Windows 10 및 macOS (Apple Silicon 포함) 모두에서 작동합니다.

> [!IMPORTANT]
> 현재 Xbox Gamepass/Xbox 콘솔 플레이어는 전용 서버에 참여할 수 없습니다.
>
> 초대 코드를 통해 다른 플레이어들과 함께 게임을 즐길 수 있으며, 게임은 최대 4명의 플레이어로 제한됩니다.

## 스폰서

다음 스폰서들에게 큰 박수를 보냅니다!

<p align="center"><!-- markdownlint-disable-line --><!-- markdownlint-disable-next-line -->
<!-- sponsors --><a href="https://github.com/doomhound188"><img src="https://github.com/doomhound188.png" width="50px" alt="doomhound188" /></a>&nbsp;&nbsp;<a href="https://github.com/AshishT112203"><img src="https://github.com/AshishT112203.png" width="50px" alt="AshishT112203" /></a>&nbsp;&nbsp;<a href="https://github.com/pabumake"><img src="https://github.com/pabumake.png" width="50px" alt="pabumake" /></a>&nbsp;&nbsp;<a href="https://github.com/stoprx"><img src="https://github.com/stoprx.png" width="50px" alt="stoprx" /></a>&nbsp;&nbsp;<a href="https://github.com/KiKoS0"><img src="https://github.com/KiKoS0.png" width="50px" alt="KiKoS0" /></a>&nbsp;&nbsp;<a href="https://github.com/inspired-by-nudes"><img src="https://github.com/inspired-by-nudes.png" width="50px" alt="inspired-by-nudes" /></a>&nbsp;&nbsp;<a href="https://github.com/PulsarFTW"><img src="https://github.com/PulsarFTW.png" width="50px" alt="PulsarFTW" /></a>&nbsp;&nbsp;<!-- sponsors -->
</p>

## Official Documentation

[![Documentation](https://github.com/thijsvanloef/palworld-server-docker/assets/58031337/b92d76d1-5efb-438d-9ffd-5385544a831b)](https://palworld-server-docker.loef.dev/ko/)

## 서버 요구 사양

| 리소스 | 최소    | 추천                                |
| ------ | ------- | ----------------------------------- |
| CPU    | 4 cores | 4+ cores                            |
| RAM    | 16GB    | 안정적인 운영을 위해 32GB 이상 권장 |
| 저장소 | 8GB     | 20GB                                |

## 사용하기

서버를 가동하기 위해서는 반드시 [환경 변수](#환경-변수)를 수정해야 합니다. 잊지 마세요!

> [!IMPORTANT]
> 만약 ARM64 기반의 CPU (라즈베리 파이, M1, M2, M3 Mac, 오라클 클라우드 프리 티어 등) 를 사용하고 있다면,
> 반드시 `:latest-arm64` 이미지 태그를 사용해주세요.

### Docker Compose

이 저장소에는 서버를 설정하는 데 사용할 수 있는 [docker-compose.yml](/docker-compose.yml)예제 파일이 포함되어 있습니다.

```yml
services:
  palworld:
    image: thijsvanloef/palworld-server-docker:latest
    restart: unless-stopped
    container_name: palworld-server
    stop_grace_period: 30s # 컨테이너가 정상적으로 중지될 때까지 기다리는 시간을 설정합니다.
    ports:
      - 8211:8211/udp
      - 27015:27015/udp
    environment:
      - PUID=1000
      - PGID=1000
      - PORT=8211 # 선택사항이지만 설정하는 것을 권장합니다.
      - PLAYERS=16 # 선택사항이지만 설정하는 것을 권장합니다.
      - SERVER_PASSWORD="worldofpals" # 선택사항이지만 설정하는 것을 권장합니다.
      - MULTITHREADING=true
      - RCON_ENABLED=true
      - RCON_PORT=25575
      - TZ=Asia/Seoul
      - ADMIN_PASSWORD="adminPasswordHere"
      - COMMUNITY=false # 커뮤니티 서버 탐색기에 서버가 표시 되는 것을 허용합니다 (USE WITH SERVER_PASSWORD 와 함께 사용하는 것을 권장합니다)
      - SERVER_NAME="World of Pals"
    volumes:
      - ./palworld:/palworld/
```

환경 변수를 설정하는 또 다른 방법은 **.env** 파일을 사용하는 것입니다. [.env.example](/.env.example) 파일을 **.env**라는 새 파일로 복사한 후 필요에 따라 내용을 수정하세요.
환경 변수에 대한 올바른 값을 확인하려면 [환경 변수](#환경-변수) 섹션을 참조하세요.
[docker-compose.yml](/docker-compose.yml) 파일을 다음과 같이 수정하세요:

```yml
services:
  palworld:
    image: thijsvanloef/palworld-server-docker:latest
    restart: unless-stopped
    container_name: palworld-server
    stop_grace_period: 30s # 컨테이너가 정상적으로 중지될 때까지 기다리는 시간을 설정합니다.
    ports:
      - 8211:8211/udp
      - 27015:27015/udp
    env_file:
      - .env
    volumes:
      - ./palworld:/palworld/
```

### Docker Run

모든 <>를 자신만의 구성으로 변경하세요.

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
    -e TZ=KST \
    -e ADMIN_PASSWORD=adminPasswordHere \
    -e SERVER_PASSWORD=worldofpals \
    -e COMMUNITY=false \
    -e SERVER_NAME=World of Pals \
    -e SERVER_DESCRIPTION=palworld-server-docker by Thijs van Loef \
    --restart unless-stopped \
    --stop-timeout 30 \
    thijsvanloef/palworld-server-docker:latest
```

환경 변수를 설정하는 또 다른 방법은 **.env** 파일을 사용하는 것입니다. [.env.example](/.env.example) 파일을 **.env**라는 새 파일로 복사한 후 필요에 따라 내용을 수정하세요.
환경 변수에 대한 올바른 값을 확인하려면 [환경 변수](#환경-변수) 섹션을 참조하세요. docker run 명령어를 다음과 같이 변경하세요:

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

쿠버네티스에 이 컨테이너를 배포하는 데 필요한 모든 파일은 [k8s 폴더](/k8s/)에 있습니다.

[README.md](/k8s/readme.md) 에 있는 지침을 따라 배포를 진행해주세요.

#### Helm 차트 사용하기

공식 Helm 차트는 별도의 저장소에서 찾을 수 있습니다, [palworld-server-chart](https://github.com/Twinki14/palworld-server-chart)

### 루트권한 없이 실행하기

해당 항목은 고급사용자를 위한 항목입니다.

해당 컨테이너는 [기본사용자 덮어씌우기](https://docs.docker.com/engine/reference/run/#user)가 가능합니다. 해당 이미지의 기본사용자는 루트입니다.

사용자가 유저와 그룹을 설정하기 떄문에 `PUID`와 `PGID`가 무시됩니다.

UID를 찾으려면: `id -u`를 사용합니다.
GID를 찾으려면: `id -g`를 사용합니다.

반드시 유저를 `NUMBERICAL_UID:NUMBERICAL_GID`와 같이 설정하셔야합니다.

아래는 UID를 1000으로 GID를 1001로 가정해 작성된 예제문입니다.

* docker run에서 `--user 1000:1001 \`를 마지막 줄위에 추가하세요.
* docker compose에서 `user: 1000:1001`를 포트설정위에 추가하세요.

만약 다른 UID/GID를 사용해 실행하려면 디렉토리의 소유권을 변경해야합니다: `chown UID:GID palworld/` 또는 모든 계정의 권한를 수정하셔야합니다: `chmod o=rwx palworld/`

#### helm chart 사용

공식 helm chart 사용법은 다른 리포지토리에 있습니다, [palworld-server-chart](https://github.com/Twinki14/palworld-server-chart)

### 환경 변수

다음 값을 사용하여 부팅 시 서버의 설정을 변경할 수 있습니다.
서버를 시작하기 전에 다음 환경 변수를 설정하는 것이 좋습니다:

* PLAYERS
* PORT
* PUID
* PGID

| 변수명             | 정보                                                                                                                                                     | 기본값 | 허용되는 값                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------ |
| TZ                 | 서버 백업에 사용되는 타임스템프 시간대                                                                                                                   | KST    | [TZ Identifiers](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#Time_Zone_abbreviations) |
| PLAYERS\*          | 서버에 참여할 수 있는 최대 플레이어 수                                                                                                                   | 16     | 1-32                                                                                                   |
| PORT\*             | 서버에 사용되는 포트(UDP)                                                                                                                                | 8211   | 1024-65535                                                                                             |
| PUID\*             | 서버를 실행할 사용자의 아이디입니다.                                                                                                                     | 1000   | !0                                                                                                     |
| PGID\*             | 서버가 실행해야 하는 그룹의 GID입니다.                                                                                                                   | 1000   | !0                                                                                                     |
| MULTITHREADING\*\* | 멀티 스레드 CPU 환경에서 성능을 향상시킵니다. 최대 약 4개의 스레드까지만 효과가 있으며, 이 이상의 스레드를 할당하는 것은 큰 의미가 없습니다.             | false  | true/false                                                                                             |
| COMMUNITY          | 커뮤니티 서버 탐색기에 서버가 표시되는지 여부(USE WITH SERVER_PASSWORD 와 함께 사용)                                                                     | false  | true/false                                                                                             |
| PUBLIC_IP          | 서버가 실행 중인 네트워크의 PUBLIC IP를 수동으로 지정할 수 있습니다. 지정하지 않으면 자동으로 감지됩니다. 제대로 작동하지 않으면 수동 구성을 시도하세요. |        | x.x.x.x                                                                                                |
| PUBLIC_PORT        | 서버가 실행 중인 네트워크의 포트 번호를 수동으로 지정할 수 있습니다. 지정하지 않으면 자동으로 감지됩니다. 제대로 작동하지 않으면 수동 구성을 시도하세요. |        | 1024-65535                                                                                             |
| SERVER_NAME        | 서버 이름                                                                                                                                                |        | "string"                                                                                               |
| SERVER_DESCRIPTION        | 서버 설명                                                                                                                                                |        | "string"                                                                                               |
| SERVER_PASSWORD    | 서버 접속을 위한 비밀번호                                                                                                                                |        | "string"                                                                                               |
| ADMIN_PASSWORD     | 관리자 비밀번호                                                                                                                                          |        | "string"                                                                                               |
| UPDATE_ON_BOOT\*\* | 도커 컨테이너가 시작될 때 서버 업데이트/설치(컨테이너를 처음 실행할 때 이 기능을 활성화해야 합니다).                                                     | true   | true/false                                                                                             |
| RCON_ENABLED\*\*\* | Palworld RCON 활성화                                                                                                                                     | true   | true/false                                                                                             |
| RCON_PORT          | RCON접속 포트                                                                                                                                            | 25575  | 1024-65535                                                                                             |
| QUERY_PORT         | Steam 서버와 통신하는 데 사용되는 쿼리 포트                                                                                                              | 27015  | 1024-65535                                                                                             |
| BACKUP_CRON_EXPRESSION  | 자동 백업 주기 | 0 0 \* \* \* | Cron 표현식 필요 - [cron을 이용한 자동 백업 설정](#cron을-이용한-자동-백업-설정) 참조 |
| BACKUP_ENABLED | 자동 백업을 활성화 여부 | true | true/false |
| DELETE_OLD_BACKUPS | 오래된 백업 파일 자동 삭제 여부                                                                                                                                                       | false          | true/false                                                                                                 |
| OLD_BACKUP_DAYS    | 백업 보관 일수                                                                                                                                                                       | 30             | 임의의 양의 정수                                                                                       |
| AUTO_UPDATE_CRON_EXPRESSION  | 자동 업데이트 주기. | 0 \* \* \* \* | Cron 표현식 필요 - [cron을 이용한 자동 업데이트 설정](#cron을-이용한-자동-업데이트-설정) 참조 |
| AUTO_UPDATE_ENABLED | 자동 업데이트 활성화 여부 | false | true/false |
| AUTO_UPDATE_WARN_MINUTES | 업데이트 대기 시간 설정(분), 이때 사용자는 분 단위로 서버 업데이트에 대한 알림을 받습니다 | 30 | !0 |
| AUTO_REBOOT_CRON_EXPRESSION  | 자동 서버 재부팅 주기 | 0 0 \* \* \* | Cron 표현식 필요 - [cron을 이용한 자동 재부팅 설정](#cron을-이용한-자동-재부팅-설정) 참조 |
| AUTO_REBOOT_ENABLED | 자동 서버 재부팅 활성화 여부 | false | true/false |
| AUTO_REBOOT_WARN_MINUTES | 재부팅 대기 시간 설정(분), 이때 사용자는 분 단위로 서버 종료에 대한 알림을 받습니다. | 5 | !0 |
| AUTO_REBOOT_EVEN_IF_PLAYERS_ONLINE | 온라인 사용자가 있어도 재시작.                                                                                                                                | false                      | true/false                                                                                                        |
| TARGET_MANIFEST_ID | 게임의 버젼을 스팀 다운로드 디포의 해당 Manifest ID로 고정.                                                                                                                       |                           | [Manifest ID Table](#버전별Manifest ID표) 보 |
| DISCORD_WEBHOOK_URL | 디스코드 웹훅 URL | | `https://discord.com/api/webhooks/<webhook_id>` |
| DISCORD_CONNECT_TIMEOUT | 디스코드 명령 초기 연결 시간 초과 | 30 | !0 |
| DISCORD_MAX_TIMEOUT | Discord 총 훅 시간 초과 | 30 | !0 |
| DISCORD_PRE_UPDATE_BOOT_MESSAGE | 서버 업데이트 시작 시 전송되는 디스코드 메시지 | Server is updating... | "string" |
| DISCORD_POST_UPDATE_BOOT_MESSAGE | 서버 업데이트 완료 시 전송되는 디스코드 메시지 | Server update complete! | "string" |
| DISCORD_PRE_START_MESSAGE | 서버가 시작될 때 전송되는 디스코드 메시지 | Server is started! | "string" |
| DISCORD_PRE_SHUTDOWN_MESSAGE | 서버가 종료되기 시작할 때 전송되는 디스코드 메시지 | Server is shutting down... | "string" |
| DISCORD_POST_SHUTDOWN_MESSAGE | 서버가 멈췄을 때 전송되는 디스코드 메시지 | Server is stopped! | "string" |
| DISABLE_GENERATE_SETTINGS | 자동으로 PalWorldSettings.ini를 생성할지 여부 | false | true/false |
| DISABLE_GENERATE_ENGINE  | 자동으로 Engine.ini를 생성할지 여부  | true                      | true/false           |
| ENABLE_PLAYER_LOGGING     | 플레이어가 접속 또는 종료시 로깅과 공지를 활성화 | true         | true/false |
| PLAYER_LOGGING_POLL_PERIOD        | 플레이어의 접속과 종료를 확인하기위한 폴링시간(초) 설정| 5               | !0 |

*설정하는 것을 적극 권장합니다.

** 이 옵션을 활성화하여 실행할 때 주의해야 할 사항을 확인하세요.

*** docker stop이 서버를 저장하고 정상적으로 종료하는 데 필요합니다.

### 사용되는 포트

| Port  | Info             |
| ----- | ---------------- |
| 8211  | Game Port (UDP)  |
| 27015 | Query Port (UDP) |
| 25575 | RCON Port (TCP)  |

## RCON 사용

RCON은 palworld-server-docker 이미지에 기본적으로 활성화되어 있습니다. RCON CLI는 아주 쉽게 열 수 있습니다:

```bash
docker exec -it palworld-server rcon-cli "<command> <value>"
```

예를 들어, 다음 명령어를 사용하여 서버의 모든 사람에게 메시지를 방송할 수 있습니다:

```bash
docker exec -it palworld-server rcon-cli "Broadcast Hello everyone"
```

위 명령어를 사용 하면 RCON을 사용하여 Palworld 서버 명령어를 작성할 수 있는 CLI가 열립니다.

### 서버 명령어 리스트

| 명령어                           | 정보                                               |
| -------------------------------- | -------------------------------------------------- |
| Shutdown {Seconds} {MessageText} | {Seconds}가 지나면 서버가 종료됩니다.              |
| DoExit                           | 서버를 강제 종료합니다.                            |
| Broadcast                        | 서버에 있는 모든 플레이어에게 메시지를 전송합니다. |
| KickPlayer {SteamID}             | 서버에서 플레이어를 추방합니다.                    |
| BanPlayer {SteamID}              | 서버에서 사용자를 차단합니다.                      |
| TeleportToPlayer {SteamID}       | 대상 플레이어의 위치로 순간이동합니다.             |
| TeleportToMe {SteamID}           | 대상 플레이어가 현재 위치로 순간이동합니다.        |
| ShowPlayers                      | 서버에 있는 모든 플레이어의 정보를 표시합니다.     |
| Info                             | 서버 정보를 표시합니다.                            |
| Save                             | 월드 정보를 저장합니다.                            |

전체 명령어 목록을 보려면 다음으로 이동하세요: [https://tech.palworldgame.com/settings-and-operation/commands](https://tech.palworldgame.com/settings-and-operation/commands)

## 백업 만들기

현재 시점의 게임 세이브 백업을 생성하려면 다음 명령을 사용합니다:

```bash
docker exec palworld-server backup
```

다음 위치에 백업이 생성됩니다. `/palworld/backups/`

rcon이 활성화된 경우 서버는 백업 전에 저장을 실행합니다.

## 백업 복원 하기

백업을 복원하려면 다음 명령을 사용합니다:

```bash
docker exec -it palworld-server restore
```

복원 명령어를 사용하려면 `RCON_ENABLED` 환경 변수가 `true`여야 합니다.

> [!IMPORTANT]
> 도커 `restart` 정책이 `always` 또는 `unless-stopped`로 설정 되어있지 않다면 복원 이후 컨테이너가 종료되므로 수동으로 재시작 해야 합니다.
>
> [사용하기](#사용하기)에서 제공된 Docker 실행 명령어와 Docker Compose 파일 예시는 이미 필요한 정책을 적용하고 있습니다.

## 수동으로 백업에서 복원하기

`/palworld/backups/`에서 복원하고자 하는 백업을 찾아서 압축을 풉니다.
작업을 시작하기 전에 서버를 중지해야 합니다.

```bash
docker compose down
```

`palworld/Pal/Saved/SaveGames/0/<old_hash_value>`에 위치한 기존 저장 데이터 폴더를 삭제합니다.

새롭게 압축 해제된 저장 데이터 폴더 `Saved/SaveGames/0/<new_hash_value>`의 내용을 `palworld/Pal/Saved/SaveGames/0/<new_hash_value>`로 복사합니다.

`palworld/Pal/Saved/Config/LinuxServer/GameUserSettings.ini` 안의 DedicatedServerName을 새 폴더 이름으로 교체합니다.

```ini
DedicatedServerName=<new_hash_value>  # 폴더 이름으로 교체하세요.
```

게임을 재시작합니다. (Docker Compose를 사용하는 경우)

```bash
docker compose up -d
```

## Cron을 이용한 자동 백업 설정

서버는 TZ로 설정된 시간대에 따라 매일 자정에 자동으로 백업됩니다.

BACKUP_ENABLED를 설정하여 자동 백업을 활성화하거나 비활성화합니다 (기본값은 활성화됨).

BACKUP_CRON_EXPRESSION은 cron 표현식으로, Cron 표현식에서는 작업을 실행할 간격을 정의합니다.

> [!TIP]
> 이 이미지는 cron 작업을 위해 Supercronic을 사용합니다.
> [supercronic](https://github.com/aptible/supercronic#crontab-format) 또는
> [Crontab Generator](https://crontab-generator.org)를 참조하세요.

BACKUP_CRON_EXPRESSION을 설정하여 기본 스케줄을 변경합니다.
예시: `0 2 * * *`로 BACKUP_CRON_EXPRESSION을 설정하면, 백업 스크립트는 매일 새벽 2시에 실행됩니다.

## Cron을 이용한 자동 업데이트 설정

이 서버에서 자동 업데이트를 사용하려면 다음 환경 변수들을 `true`로 **설정해야 합니다**:

* RCON_ENABLED
* UPDATE_ON_BOOT

> [!IMPORTANT]
> 도커 `restart` 정책이 `always` 또는 `unless-stopped`로 설정 되어있지 않다면, 서버는 종료되고
> 수동으로 다시 시작해야 합니다.
>
> [사용하기](#사용하기)에서 제공된 Docker 실행 명령어와 Docker Compose 파일 예시는 이미 필요한 정책을 적용하고 있습니다.

AUTO_UPDATE_ENABLED를 설정하여 자동 업데이트를 활성화하거나 비활성화합니다 (기본값은 비활성화됨).

AUTO_UPDATE_CRON_EXPRESSION은 cron 표현식으로, Cron 표현식에서는 작업을 실행할 간격을 정의합니다.

> [!TIP]
> 이 이미지는 cron 작업을 위해 Supercronic을 사용합니다.
> [supercronic](https://github.com/aptible/supercronic#crontab-format) 또는
> [Crontab Generator](https://crontab-generator.org)를 참조하세요.

AUTO_UPDATE_CRON_EXPRESSION을 설정하여 기본 스케줄을 변경합니다.

## Cron을 이용한 자동 재부팅 설정

이 서버에서 자동 재부팅을 사용하려면 RCON_ENABLED를 활성화해야 합니다.

> [!IMPORTANT]
> 도커 `restart` 정책이 `always` 또는 `unless-stopped`로 설정 되어있지 않다면, 서버는 종료되고
> 수동으로 다시 시작해야 합니다.
>
> [사용하기](#사용하기)에서 제공된 Docker 실행 명령어와 Docker Compose 파일 예시는 이미 필요한 정책을 적용하고 있습니다.

AUTO_REBOOT_ENABLED를 설정하여 자동 재부팅을 활성화하거나 비활성화합니다 (기본값은 비활성화됨).

AUTO_REBOOT_CRON_EXPRESSION은 cron 표현식으로, Cron 표현식에서는 작업을 실행할 간격을 정의합니다.

> [!TIP]
> 이 이미지는 cron 작업을 위해 Supercronic을 사용합니다.
> [supercronic](https://github.com/aptible/supercronic#crontab-format) 또는
> [Crontab Generator](https://crontab-generator.org)를 참조하세요.

AUTO_REBOOT_CRON_EXPRESSION을 설정하여 기본 스케줄을 변경하세요. 기본 설정은 TZ로 설정된 시간대에 따라 매일 자정에 재부팅됩니다.

## 서버 설정 편집

### 환경 변수 사용 설정

> [!IMPORTANT]
>
> 게임이 아직 베타버전이므로 이러한 환경 변수/설정은 변경될 수 있습니다. 지원되는 파라미터들을 [공식 웹페이지](https://tech.palworldgame.com/optimize-game-balance)에서 확인하세요.

서버 설정을 환경 변수로 바꾸는 과정은 다음과 같은 규칙을 따릅니다 (몇가지 예외 있음):

* 모두 대문자로 작성
* 밑줄을 삽입하여 단어를 분할
* 한 글자로 시작하는 설정(예: 'b')의 경우 그 한 글자를 제거

아래는 예시입니다:

* Difficulty -> DIFFICULTY
* PalSpawnNumRate -> PAL_SPAWN_NUM_RATE
* bIsPvP -> IS_PVP

| 변수                                  | 설명                                                    | 기본값                                                                                | 허용값                          |
|-------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------------------------------------|----------------------------------------|
| DIFFICULTY                                | 게임 난이도                                                | None                                                                                         | `None`,`Normal`,`Difficult`            |
| DAYTIME_SPEEDRATE                         | 낮 시간 속도 - 숫자가 작을수록 낮이 짧아짐             | 1.000000                                                                                     | Float                                  |
| NIGHTTIME_SPEEDRATE                       | 밤 시간 속도 - 숫자가 작을수록 밤이 짧아짐         | 1.000000                                                                                     | Float                                  |
| EXP_RATE                                  | 경험치 획득 비율                                                 | 1.000000                                                                                     | Float                                  |
| PAL_CAPTURE_RATE                          | PAL 포획률                                               | 1.000000                                                                                     | Float                                  |
| PAL_SPAWN_NUM_RATE                        | PAL 출현 비율                                            | 1.000000                                                                                     | Float                                  |
| PAL_DAMAGE_RATE_ATTACK                    | PAL이 주는 데미지 배수                                    | 1.000000                                                                                     | Float                                  |
| PAL_DAMAGE_RATE_DEFENSE                   | PAL이 받는 데미지 배수                                      | 1.000000                                                                                     | Float                                  |
| PLAYER_DAMAGE_RATE_ATTACK                 | 플레이어가 주는 데미지 배수                                  | 1.000000                                                                                     | Float                                  |
| PLAYER_DAMAGE_RATE_DEFENSE                | 플레이어가 받는 데미지 배수                                   | 1.000000                                                                                     | Float                                  |
| PLAYER_STOMACH_DECREASE_RATE              | 플레이어 포만도 감소율                                   | 1.000000                                                                                     | Float                                  |
| PLAYER_STAMINA_DECREASE_RATE              | 플레이어 기력 감소율                                 | 1.000000                                                                                     | Float                                  |
| PLAYER_AUTO_HP_REGEN_RATE                 | 플레이어 HP 자연 회복률                               | 1.000000                                                                                     | Float                                  |
| PLAYER_AUTO_HP_REGEN_RATE_IN_SLEEP        | 플레이어 수면 시 HP 회복률                              | 1.000000                                                                                     | Float                                  |
| PAL_STOMACH_DECREASE_RATE                 | PAL 포만도 감소율                                      | 1.000000                                                                                     | Float                                  |
| PAL_STAMINA_DECREASE_RATE                 | PAL 기력 감소율                                     | 1.000000                                                                                     | Float                                  |
| PAL_AUTO_HP_REGEN_RATE                    | PAL HP 자연 회복률                                  | 1.000000                                                                                     | Float                                  |
| PAL_AUTO_HP_REGEN_RATE_IN_SLEEP           | PAL 수면 시 HP 회복률 (PLA상자 내 HP 회복률)                 | 1.000000                                                                                     | Float                                  |
| BUILD_OBJECT_DAMAGE_RATE                  | 구조물 피해 배수                                 | 1.000000                                                                                     | Float                                  |
| BUILD_OBJECT_DETERIORATION_DAMAGE_RATE    | 구조물 노화 속도 배수                                   | 1.000000                                                                                     | Float                                  |
| COLLECTION_DROP_RATE                      | 채집 아이템 획득량 배수                                    | 1.000000                                                                                     | Float                                  |
| COLLECTION_OBJECT_HP_RATE                 | 채집 오브젝트 HP 배수                               | 1.000000                                                                                     | Float                                  |
| COLLECTION_OBJECT_RESPAWN_SPEED_RATE      | 채집 오브젝트 생성 간격 - 숫자가 작을수록 재 생성이 빨라짐                           | 1.000000                                                                                     | Float                                  |
| ENEMY_DROP_ITEM_RATE                      | 드롭 아이템 양 배수                                       | 1.000000                                                                                     | Float                                  |
| DEATH_PENALTY                             | 사망 패널티</br>None: 사망 패널티 없음</br>Item: 장비 이외의 아이템 드롭</br>ItemAndEquipment: 모든 아이템 드롭</br>All: 모든 PAL과 모든 아이템 드롭                                   | All                                                                                          | `None`,`Item`,`ItemAndEquipment`,`All` |
| ENABLE_PLAYER_TO_PLAYER_DAMAGE            | 플레이어간 데미지 여부                      | False                                                                                        | Boolean                                |
| ENABLE_FRIENDLY_FIRE                      | 아군간 데미지 여부                                            | False                                                                                        | Boolean                                |
| ENABLE_INVADER_ENEMY                      | 습격 이벤트 발생 여부                                                | True                                                                                         | Boolean                                |
| ACTIVE_UNKO                               | UNKO 활성화 여부(?)                                                | False                                                                                        | Boolean                                |
| ENABLE_AIM_ASSIST_PAD                     | 컨트롤러 조준 보조 활성화                                   | True                                                                                         | Boolean                                |
| ENABLE_AIM_ASSIST_KEYBOARD                | 키보드 조준 보조 활성화                                     | False                                                                                        | Boolean                                |
| DROP_ITEM_MAX_NUM                         | 월드 내의 드롭 아이템 최대 수                           | 3000                                                                                         | Integer                                |
| DROP_ITEM_MAX_NUM_UNKO                    | 월드 내의 UNKO 드롭 최대 수                      | 100                                                                                          | Integer                                |
| BASE_CAMP_MAX_NUM                         | 거점 최대 수량                                   | 128                                                                                          | Integer                                |
| BASE_CAMP_WORKER_MAX_NUM                  | 거점 작업 PAL 최대 수                                      | 15                                                                                           | Integer                                |
| DROP_ITEM_ALIVE_MAX_HOURS                 | 드롭 아이템이 사라지기까지 걸리는 시간                    | 1.000000                                                                                     | Float                                  |
| AUTO_RESET_GUILD_NO_ONLINE_PLAYERS        | 온라인 플레이어가 없을 때 길드 자동 리셋 여부          | False                                                                                        | Bool                                   |
| AUTO_RESET_GUILD_TIME_NO_ONLINE_PLAYERS   | 온라인 플레이어가 없을 때 길드를 자동 리셋 시간(h)   | 72.000000                                                                                    | Float                                  |
| GUILD_PLAYER_MAX_NUM                      | 길드 내 최대 인원 수                                            | 20                                                                                           | Integer                                |
| PAL_EGG_DEFAULT_HATCHING_TIME             | 거대알 부화에 걸리는 시간(h)                                | 72.000000                                                                                    | Float                                  |
| WORK_SPEED_RATE                           | 작업 속도 배수                                          | 1.000000                                                                                     | Float                                  |
| IS_MULTIPLAY                              | 멀티플레이 활성화 여부                                             | False                                                                                        | Boolean                                |
| IS_PVP                                    | PVP 활성화 여부                                                     | False                                                                                        | Boolean                                |
| CAN_PICKUP_OTHER_GUILD_DEATH_PENALTY_DROP | 다른 길드 플레이어의 데스 페널티 드롭 아이템 획득 가능 여부 | False                                                                                        | Boolean                                |
| ENABLE_NON_LOGIN_PENALTY                  | 비 로그인 패널티 활성화 여부                                       | True                                                                                         | Boolean                                |
| ENABLE_FAST_TRAVEL                        | 빠른 이동 활성화 여부                                             | True                                                                                         | Boolean                                |
| IS_START_LOCATION_SELECT_BY_MAP           | 시작 위치를 지도로 선택할 수 있는지 여부                             | True                                                                                         | Boolean                                |
| EXIST_PLAYER_AFTER_LOGOUT                 | 로그오프 후 플레이어 삭제 여부                  | False                                                                                        | Boolean                                |
| ENABLE_DEFENSE_OTHER_GUILD_PLAYER         | 다른 길드 플레이어에 대한 방어 허용 여부                     | False                                                                                        | Boolean                                |
| COOP_PLAYER_MAX_NUM                       | 협동던전 최대인원                           | 4                                                                                            | Integer                                |
| REGION                                    | Region                                                         |                                                                                              | String                                 |
| USEAUTH                                   | 인증 사용 여부                                             | True                                                                                         | Boolean                                |
| BAN_LIST_URL                              | 사용할 BAN 목록                                          | [https://api.palworldgame.com/api/banlist.txt](https://api.palworldgame.com/api/banlist.txt) | string                                 |
| SHOW_PLAYER_LIST                          | Enable show player list                                        | True                                                                                         | Boolean                                |

### 수동 설정

서버가 시작되면 `PalWorldSettings.ini` 파일은 다음 위치에 생성됩니다.
`<mount_folder>/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`

환경 변수 설정은 항상 `PalWorldSettings.ini`의 변경 사항을 덮어쓴다는 점에 유의하세요.

> [!IMPORTANT]
> 서버가 꺼져 있을 때만 `PalWorldSettings.ini`를 변경할 수 있습니다.
>
> 서버가 작동하는 동안 변경한 내용은 서버가 중지되면 덮어쓰기됩니다.

서버 설정에 대한 자세한 목록을 보려면 다음을 참조하세요: [Palworld Wiki](https://palworld.wiki.gg/wiki/PalWorldSettings.ini)

서버 설정에 대한 자세한 설명을 보려면 다음을 참조하세요: [shockbyte](https://shockbyte.com/billing/knowledgebase/1189/How-to-Configure-your-Palworld-server.html)

> [!TIP]
> 만약 `<mount_folder>/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini` 파일 내부가 비어 있으면,
> 파일을 삭제하고 서버를 다시 시작하면 콘텐츠가 포함된 새 파일이 생성됩니다.

## 디스코드 웹훅 사용하기

1. 디스코드의 서버 설정 창에서 웹훅 url을 생성하세요.
2. 끝 부분에 고유 토큰이 있는 디스코드 웹훅 url을 환경 변수로 설정하세요.  
(예시: <https://discord.com/api/webhooks/1234567890/abcde>)

docker run으로 디스코드 메시지 보내기:

```sh
-e DISCORD_WEBHOOK_URL="https://discord.com/api/webhooks/1234567890/abcde" \
-e DISCORD_PRE_UPDATE_BOOT_MESSAGE="Server is updating..." \
```

docker compose로 디스코드 메시지 보내기:

```yaml
- DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/1234567890/abcde
- DISCORD_PRE_UPDATE_BOOT_MESSAGE=Server is updating...
```

## 게임버전 고정하기

>[!WARNING]
>다운그레이딩이 가능하나 세이브 파일에 어떠한 영향이 있을지 알 수 없습니다.
>
>**본인의 책임하에 하세요!**

만약 **TARGET_MANIFEST_ID** 환경변수가 정해졌다면, 서버의 버전은 특정 manifest로 고정됩니다.
Manifest는 게임의 릴리즈 시간/업데이트 버젼에 따라 정해집니다. Manifest는 SteamCMD나 [SteamDB](https://steamdb.info/depot/2857201/manifests/)와
같은 웹사이트에서 검색가능합니다.

### 버전별 Manifest ID 표

| Version | Manifest ID         |
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

## 이슈/기능 요청

문제/기능 요청은 다음 [링크](https://github.com/thijsvanloef/palworld-server-docker/issues/new/choose)에서 제출할 수 있습니다.

### 알려진 이슈

알려진 이슈는 [Wiki](https://github.com/thijsvanloef/palworld-server-docker/wiki/Known-Issues)에 나열되어 있습니다.
