# irc-relay-server

![Language](https://img.shields.io/badge/language-C%2B%2B17-blue?logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey)

42 `ft_irc` 과제를 변형한 C++17 프로젝트입니다. 단일 프로세스에서 논블로킹 TCP 연결을 다중화해 IRC 클라이언트의 등록, 개인 메시지, 채널 메시지를 중계합니다.

## 빌드

```sh
make
```

산출물은 `build/bin/irc-relay-server`입니다. 이벤트 백엔드는 Linux에서 `epoll`, macOS에서 `kqueue`로 정해집니다.

## 실행

```sh
./build/bin/irc-relay-server 6667 relay-secret --rate-limit=24:3 --max-pending-bytes=1048576
```

첫 두 인자는 포트(1–65535)와 접속 비밀번호입니다. 옵션은 `--idle-timeout=N`(기본 120), `--ping-timeout=N`(기본 30), `--registration-timeout=N`(기본 30), `--rate-limit=COUNT:SECONDS`(기본 `24:3`), `--max-pending-bytes=N`(기본 1048576), `--max-connections=N`(기본 256)이며 시간 단위는 초입니다.

## 테스트

```sh
make test
```

- `make connection-test`, `make unit`, `make application-test`: 연결, 서버 수명, 애플리케이션 수명 단위 테스트를 하나씩 실행합니다.
- `make event-test`: 동시 연결 160개로 이벤트 공정성을 검사합니다. `make test`의 스모크와 이 타깃은 `python3`가 필요합니다.

## 범위와 제한

- 처리하는 명령은 `PASS`, `NICK`, `USER`, `PING`, `PONG`, `QUIT`, `PRIVMSG`, `JOIN`, `PART`, `TOPIC`, `KICK`, `INVITE`, `MODE`, `LIST`, `NAMES`와 서버 통계용 `METRICS`입니다. 클라이언트가 보낸 `NOTICE`는 `421`로 거절합니다.
- 채널 모드는 `i`, `t`, `o`만 지원합니다. `k`(채널 키)와 `l`(인원 제한)은 `472`로 거절합니다.
- `0.0.0.0`에 IPv4로만 listen합니다. TLS와 서버 간 연결은 없습니다.
- Linux와 macOS 외의 OS에서는 Makefile이 빌드를 거부합니다.
