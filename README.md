# cpp-workspace

![Language](https://img.shields.io/badge/language-C%2B%2B98%20%7C%20C%2B%2B17-blue?logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/platform-POSIX-lightgrey)
![Build](https://img.shields.io/badge/build-Make%20%7C%20CMake-lightgrey)

42 공통 과정의 C++ 과제들을 각각 변형해 다시 구현한 저장소입니다. 프로젝트마다 독립된 브랜치를 쓰며, 브랜치끼리 공통 조상이 없는 별개의 루트 히스토리입니다. 이 브랜치(`main`)에는 이 인덱스 문서만 있습니다.

## 프로젝트

| 프로젝트 | 원 과제 | 표준 | 설명 |
| --- | --- | --- | --- |
| [cpp-foundation](https://github.com/seungwoo7050/cpp-workspace/tree/cpp-foundation) | `CPP00`–`CPP09` | C++98 | 객체 수명 · 복사 의미론 · 다형성 · 변환 경계 · 템플릿과 예외 안전성을 다루는 정적 라이브러리와 예제 프로그램 모음 |
| [ray-scene-tracer](https://github.com/seungwoo7050/cpp-workspace/tree/ray-scene-tracer) | `miniRT` | C++17 | `.rt` 장면을 파싱해 구 · 평면 · 원기둥을 ray tracing하고 PPM으로 출력하는 CPU 렌더러, BVH 가속과 멀티스레드 타일 렌더링 포함 |
| [irc-relay-server](https://github.com/seungwoo7050/cpp-workspace/tree/irc-relay-server) | `ft_irc` | C++17 | 단일 프로세스에서 논블로킹 TCP 연결을 다중화해 등록 · 개인 메시지 · 채널 메시지를 중계하는 IRC 서버 |

표의 순서는 과제 진행 순서를 따릅니다. 각 브랜치의 `README.md`에 해당 프로젝트의 지원 범위, 빌드, 실행, 테스트가 정리되어 있습니다.

## 브랜치 받기

```sh
git clone https://github.com/seungwoo7050/cpp-workspace.git
cd cpp-workspace
git switch irc-relay-server # 원하는 프로젝트 브랜치
```

브랜치를 바꾸면 작업 트리 전체가 해당 프로젝트로 교체됩니다. 여러 프로젝트를 동시에 열어두려면 worktree를 쓰는 편이 편합니다.

```sh
git worktree add ../irc-relay-server irc-relay-server
```

## 공통 규약

모든 프로젝트가 같은 규칙을 따릅니다.

- `-Wall -Wextra -Werror` 기준으로 경고 없이 빌드합니다. `cpp-foundation`은 C++98 범위를 `-Wpedantic -pedantic-errors`와 `-Wold-style-cast`까지 포함해 강제하고, 나머지 두 프로젝트는 C++17을 씁니다.
- 디렉터리 구조는 `include/`(공개 헤더), `src/`(구현), `tests/`(테스트), `build/`(산출물)로 통일되어 있습니다. 실행 파일은 `build/bin/`, 라이브러리는 `build/lib/`에 모이며 저장소 루트를 더럽히지 않습니다.
- 빌드와 테스트는 저장소 루트의 `Makefile` 하나로 실행합니다. `ray-scene-tracer`는 그 Makefile이 CMake build tree를 감싸므로 `cmake -S . -B build`로 직접 빌드해도 산출물 계약이 같습니다.

```sh
make      # 라이브러리 또는 실행 파일 빌드
make test # 테스트 빌드 후 즉시 실행, 실패 시 0이 아닌 종료 코드
make re   # 전체 재빌드
```

프로젝트마다 세분화된 타깃이 있습니다. `cpp-foundation`은 `make test-unit`, `failure-test`, `test-contract`, `test-integration`, `test-property`, `check`를, `irc-relay-server`는 `make connection-test`, `unit`, `application-test`, `event-test`를, `ray-scene-tracer`는 `make check`, `ci`, `help`를 제공합니다.

sanitizer는 `cpp-foundation`의 `make test-asan` · `make test-ubsan` · `make test-sanitize`, `ray-scene-tracer`의 `make sanitize`(별도 build tree)로 실행합니다.

할당 실패, 시스템 호출 실패, 부분 송신 같은 오류 경로는 추측이 아니라 fault injection으로 직접 유발해 검증합니다. 테스트는 성공 · 실패를 종료 코드로 알리므로 자동화에 그대로 쓸 수 있습니다.

## devlog

각 브랜치의 `devlog/`에는 그 프로젝트를 진행하며 정리한 노트가 있습니다.

- `devlog/01-*.md`, `02-*.md`, `03-*.md`: 해당 과제에서 가장 무게가 실린 주제를 다루는 본문. 예를 들어 `cpp-foundation`의 copy-and-swap과 값 불변식, `ray-scene-tracer`의 BVH 가속과 멀티스레드 타일 렌더링, `irc-relay-server`의 논블로킹 송신 큐와 reactor 플랫폼 추상화.
- `devlog/appendix/`: 본문에 담기엔 산발적이었던 개념을 한 편씩 독립 문서로 정리한 부록. 각 문서는 그 자체로 완결되게 읽을 수 있습니다.

부록은 난이도별 디렉터리로 나뉘며, 신입 · 주니어 채용 실무에서 자주 다뤄지는 항목은 `lv2/` 대신 `lv2-core/`에 모아두었습니다. 읽는 순서와 항목 목록은 각 브랜치의 `devlog/appendix/README.md`에 있습니다.
