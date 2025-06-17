# ray-scene-tracer

![Language](https://img.shields.io/badge/language-C%2B%2B17-blue?logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/platform-Linux%20%7C%20macOS-lightgrey)

42 `miniRT` 과제를 변형한 C++17 프로젝트입니다. `.rt` 장면 파일을 읽어 구, 평면, 원기둥을 CPU에서 ray tracing하고 PPM 이미지로 저장합니다.

## 빌드

```sh
make
```

산출물은 `build/bin/ray-scene-tracer`, `build/bin/ray-benchmark`, `build/lib/libraycore.a`입니다. Makefile은 `cmake -S . -B build`와 `cmake --build build`를 감싸며, 기본 빌드 형식은 `Release`입니다.

## 실행

```sh
./build/bin/ray-scene-tracer scenes/basic.rt out.ppm --accel bvh --threads auto --max-depth 4 --checksum
```

첫 두 인자는 장면 파일과 출력 경로입니다. 옵션은 `--accel linear|bvh`(기본 `bvh`), `--threads N|auto`(기본 `auto`), `--max-depth 0..32`(기본 4), `--checksum`(이미지 checksum을 표준 출력에 출력)입니다. 장면 파일은 `R`, `A`, `C`가 한 번씩 필요하고 `L`, `sp`, `pl`, `cy`를 줄마다 하나씩 쓰며, 예시는 `scenes/basic.rt`에 있습니다.

## 테스트

```sh
make test
```

- `make sanitize`: AddressSanitizer와 UBSan을 켠 `build/sanitize`에서 같은 테스트를 실행합니다.
- `./build/bin/ray-benchmark`: 코드로 만든 장면을 렌더링하고 결과를 JSON으로 출력합니다.

## 범위와 제한

- 도형은 구, 평면, 유한 원기둥만 지원하고 재질은 `diffuse`와 `metal`만 지원합니다.
- 픽셀당 샘플은 1개이며 안티앨리어싱은 없습니다.
- 출력 형식은 P3 PPM 하나이고 창을 띄우지 않습니다.
- CI는 Linux와 macOS에서 빌드와 테스트를 실행합니다.

## devlog

`devlog/`에 개발 기록이 있습니다. 모든 커밋이 아니라 웹 개발과 기초 CS로 이어지는 커밋만 골라 담았고, 읽는 법과 목차는 `devlog/01-*.md` 맨 위에 있습니다.
