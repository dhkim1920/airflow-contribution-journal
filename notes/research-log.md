# 개발 환경, 코드 탐색, 테스트 정리

Airflow 기여에서 가장 중요한 준비는 “코드를 어떻게 바꿀까”보다 먼저 **어떤 환경에서 확인할까**를 정하는 것입니다. 공식 문서 기준으로 초보자는 빠른 시작 경로를, 익숙한 개발자는 전체 개발 환경을 선택하면 됩니다.

## 개발 환경 선택

### Breeze

Airflow 공식 기여 문서에서 가장 자주 안내하는 로컬 개발 방식 중 하나입니다.

적합한 경우:

- 로컬에서 재현하고 싶을 때
- CI와 가까운 환경이 필요할 때
- 통합 테스트까지 염두에 둘 때

기본 흐름:

```bash
git clone https://github.com/<your-id>/airflow.git
cd airflow
git remote add upstream https://github.com/apache/airflow.git
./scripts/tools/setup_breeze
breeze start-airflow
```

공식 quick start에서는 `uv`, `prek`(정적 체크용 훅 도구), Docker/Podman, Docker Compose 준비를 먼저 안내합니다.

### GitHub Codespaces

로컬 환경 세팅이 부담될 때 좋습니다.

적합한 경우:

- 개발용 로컬 머신 자원이 부족할 때
- 빠르게 문서 수정이나 작은 코드 수정부터 시작할 때
- 브라우저 기반으로 바로 진입하고 싶을 때

다만 Breeze 계열 도구를 여전히 쓰게 되므로, 완전히 “아무 세팅도 필요 없는” 흐름은 아닙니다.

## 필수 도구

### prek

Airflow는 로컬 정적 체크를 미리 돌리는 흐름을 강하게 권장합니다.

```bash
uv tool install prek
prek install -f
prek install -f --hook-type pre-push
```

이걸 해두면 커밋 전/푸시 전 품질 문제를 빨리 잡을 수 있습니다.

### upstream / origin 원칙

Airflow 문서의 기본 전제는 remote 이름이 아래처럼 맞는 것입니다.

- `upstream` = `apache/airflow`
- `origin` = 내 포크

즉, fetch는 `upstream`, push는 `origin`으로 생각하면 됩니다.

## 코드 읽기 시작점

작업 전에는 아래 세 가지를 먼저 찾는 것이 좋습니다.

1. 수정 대상 구현 파일
2. 이미 있는 관련 테스트 파일
3. 문서나 뉴스프래그먼트가 필요한지 여부

Airflow 공식 workflow 문서는 예시 이슈를 설명할 때 **구현 파일 + 테스트 파일**을 함께 확인하는 흐름을 보여줍니다.

## 테스트 감각 잡기

Airflow 테스트는 종류가 많습니다.

- unit tests
- integration tests
- docker compose tests
- kubernetes tests
- helm unit tests
- system tests

처음에는 보통 **unit test 중심**으로 생각하면 됩니다. 문서-only 변경이 아니라면 관련 unit test를 함께 보는 쪽이 안전합니다.

## 로컬에서 먼저 할 것

작은 변경이라도 보통 아래 순서를 추천합니다.

```bash
prek --all-files
```

그 다음 변경 영역에 맞는 테스트를 돌립니다.

```bash
breeze testing core-tests
```

실제로는 전체 `core-tests`보다 더 좁게 돌릴 수 있으면 좁히는 편이 좋지만, 처음에는 어떤 테스트 묶음이 맞는지 파악하는 것부터 시작해도 괜찮습니다.

## 문서 변경만 하는 경우

문서-only PR이면 코드 테스트 전체를 다 돌리는 것보다 아래를 우선합니다.

- 관련 문서 수정이 맞는지
- 링크/설명이 최신인지
- 정적 체크가 깨지지 않는지
- 필요 시 공식 문서 빌드 가이드를 따라 검증 명령을 확인하는지

예를 들어 문서-only 변경에서는 `prek --all-files`를 먼저 돌리고, 그다음 공식 문서 빌드 가이드를 열어 현재 변경에 맞는 문서 검증 절차를 추가로 확인하는 식으로 접근하면 됩니다.

## 조사 메모 템플릿

```md
### 작업 주제

- 관련 이슈:
- 수정 대상 구현 파일:
- 같이 볼 테스트 파일:
- 필요한 환경: Breeze / Codespaces / local virtualenv
- 먼저 돌릴 체크:
- 먼저 돌릴 테스트:
- 문서 업데이트 필요 여부:
- 뉴스프래그먼트 필요 여부(기능/수정 성격이면 공식 workflow 문서 확인):
```
