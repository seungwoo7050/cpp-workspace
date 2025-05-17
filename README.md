# cpp-foundation

![Language](https://img.shields.io/badge/language-C%2B%2B98-blue?logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/platform-POSIX-lightgrey)

42 `CPP00`–`CPP09` 과제를 변형한 C++98 프로젝트입니다. 객체 수명, 복사 대입, 다형성, 형 변환, 템플릿과 예외 안전성을 다루는 정적 라이브러리와 그 라이브러리를 쓰는 예제 프로그램 6개를 만듭니다.

## 빌드

```sh
make
```

산출물은 `build/lib/libcpp_foundation.a`와 `build/bin/ex00_contact_book` … `build/bin/ex05_batch_engine`입니다.

## 실행

```sh
./build/bin/ex00_contact_book < tests/fixtures/contact-session.in
./build/bin/ex03_pipeline_factory mixed 'prefix=[' upper 'suffix=]'
./build/bin/ex04_type_boundary scalar 42.5
./build/bin/ex05_batch_engine rpn '3 4 +'
```

`ex00`과 `ex05 batch`는 표준 입력에서 한 줄씩 명령을 읽고, 나머지는 인자를 받습니다. 인자 없이 실행하면 각 프로그램이 usage를 출력합니다.

## 테스트

```sh
make test
```

- `make test-ubsan`, `make test-asan`: sanitizer 빌드로 단위 테스트를 실행합니다.
- `make check`: `test`에 결정성, 정적 라이브러리 구성, Mach-O 의존성, `leaks` 검사를 더합니다.

## 범위와 제한

- `-std=c++98 -pedantic-errors -Wold-style-cast`로 빌드하므로 C++11 이후 기능은 쓰지 않습니다.
- `make check`의 정적 라이브러리·의존성·누수 검사는 macOS에서만 동작합니다.
- `make test-asan`은 `detect_leaks=1`을 켜므로 macOS의 Apple clang에서는 실행 직후 중단됩니다.
- 예제 프로그램은 라이브러리 API를 호출해 보는 용도이며 독립된 도구가 아닙니다.
