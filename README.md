# 42cpp-workspace

![Language](https://img.shields.io/badge/language-C%2B%2B98%20%7C%20C%2B%2B17-blue?logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey)
![Build](https://img.shields.io/badge/build-Make%20%7C%20CMake-lightgrey)

42 공통 과정의 C++ 과제를 변형해 다시 구현한 프로젝트를 모은 저장소입니다. 프로젝트마다 공통 조상이 없는 독립 브랜치를 쓰고, 이 브랜치(`main`)에는 이 문서만 있습니다.

## 프로젝트

| 프로젝트 | 원 과제 | 설명 |
| --- | --- | --- |
| [cpp-foundation](https://github.com/seungwoo7050/42cpp-workspace/tree/cpp-foundation) | `CPP00`–`CPP09` | 객체 수명, 복사 대입, 다형성, 형 변환, 템플릿을 다루는 C++98 정적 라이브러리와 예제 프로그램 6개 |
| [ray-scene-tracer](https://github.com/seungwoo7050/42cpp-workspace/tree/ray-scene-tracer) | `miniRT` | `.rt` 장면의 구, 평면, 원기둥을 BVH와 멀티스레드로 ray tracing해 PPM으로 저장하는 C++17 렌더러 |
| [irc-relay-server](https://github.com/seungwoo7050/42cpp-workspace/tree/irc-relay-server) | `ft_irc` | 단일 프로세스에서 논블로킹 TCP 연결을 다중화해 등록, 개인 메시지, 채널 메시지를 중계하는 C++17 IRC 서버 |

## 브랜치 받기

```sh
git clone https://github.com/seungwoo7050/42cpp-workspace.git
cd 42cpp-workspace
git switch irc-relay-server
git worktree add ../ray-scene-tracer ray-scene-tracer
```

## 공통 규약

- 빌드와 테스트는 저장소 루트의 `Makefile`로 실행합니다(`make`, `make test`, `make clean`, `make re`). `ray-scene-tracer`의 Makefile은 CMake를 감쌉니다.
- 실행 파일은 `build/bin/`에 생깁니다.
- `cpp-foundation`은 C++98(`-pedantic-errors`), 나머지 두 프로젝트는 C++17로 빌드합니다.
- 모두 `-Wall -Wextra`로 빌드하고, `-Werror`는 `cpp-foundation`과 `irc-relay-server`만 씁니다.
