# Apache Airflow `contributing-docs` 전체 정리

이 문서는 `https://github.com/apache/airflow/tree/main/contributing-docs` 하위 문서를 **항목별로 다시 묶어 정리한 장문 요약본**입니다.
목적은 원문 링크를 하나씩 타고 들어가지 않아도, Airflow가 기여자를 어떻게 온보딩하고 어떤 운영 규칙으로 프로젝트를 굴리는지 한 번에 이해할 수 있게 만드는 것입니다.

> 기준: `contributing-docs` 아래의 문서 파일들과 `quick-start-ide/`, `testing/`, `mprocs/` 하위 문서를 읽고 정리했습니다. 각 경로의 `images/` 계열 디렉터리는 설명용 자산이라 내용 요약 대상에서는 제외했습니다.

---

## 1. 이 문서 묶음 전체가 하는 일

`contributing-docs/README.rst`는 전체 기여 문서 세트의 인덱스입니다.

핵심 메시지는 단순합니다.

- 초보자는 **15분 PR 가이드**부터 시작한다.
- 더 깊게 들어가려면 **개발 환경, 정적 체크, 테스트, Git, PR, providers, docs, API, DB migration** 순으로 문서를 따라가면 된다.
- Airflow는 단일 Python 패키지 프로젝트가 아니라 **여러 distribution이 공존하는 모노레포**라서, 기여 흐름도 그 복잡도를 반영한다.

즉 이 subtree는 “PR 올리는 법”만 적은 짧은 튜토리얼 모음이 아니라, **Airflow 커뮤니티와 코드베이스를 제대로 다루기 위한 운영 문서 세트**라고 보는 것이 맞습니다.

---

## 2. 문서 구조 한눈에 보기

대략 아래처럼 나눠 볼 수 있습니다.

- **프로젝트/커뮤니티 이해**
  - `README.rst`
  - `01_roles_in_airflow_project.rst`
  - `02_how_to_communicate.rst`
- **빠른 시작 / 첫 PR 경로**
  - `03a_contributors_quick_start_beginners.rst`
  - `03_contributors_quick_start.rst`
  - `18_contribution_workflow.rst`
- **기여 정책 / PR / Git 규칙**
  - `04_how_to_contribute.rst`
  - `05_pull_requests.rst`
  - `10_working_with_git.rst`
- **개발 환경 / 도구**
  - `06_development_environments.rst`
  - `07_local_virtualenv.rst`
  - `08_static_code_checks.rst`
  - `15_node_environment_setup.rst`
  - `20_debugging_airflow_components.rst`
  - `21_keycloak_client_settings.rst`
  - `mprocs/`
- **테스트 체계**
  - `09_testing.rst`
  - `testing/*`
- **문서/설계/코어 심화 주제**
  - `11_documentation_building.rst`
  - `14_metadata_database_updates.rst`
  - `16_adding_api_endpoints.rst`
  - `17_architecture_diagrams.rst`
  - `19_execution_api_versioning.rst`
  - `24_proposing_best_practices_and_air_rules.rst`
- **provider / dependency / release 성격 문서**
  - `12_provider_distributions.rst`
  - `13_airflow_dependencies_and_extras.rst`
  - `23_provider_hook_migration_to_yaml.rst`
- **maintainer 운영 문서**
  - `25_maintainer_pr_triage.md`
- **IDE / remote dev 보조 문서**
  - `quick-start-ide/*`

---

## 3. 프로젝트 구조와 커뮤니티 역할

### `01_roles_in_airflow_project.rst`

이 문서는 Airflow 커뮤니티 안의 역할을 설명합니다.

- **PMC Member**: Apache 차원의 프로젝트 운영 원칙과 절차를 책임집니다.
- **Committer / Maintainer**: GitHub 저장소에 write access가 있고, PR 리뷰와 머지를 담당합니다.
- **Release Manager**: Airflow 본체, providers, Helm chart, Python client 등의 릴리스를 준비합니다.
- **Contributor**: 코드, 문서, 테스트, 아이디어 등 어떤 형태로든 기여하는 사람입니다.
- **Security Team**: 보안 이슈를 다루는 별도 그룹으로, 비공개 논의 규칙과 참여 조건이 매우 명확합니다.

이 문서에서 중요한 건 Airflow가 그냥 “GitHub 오픈소스 저장소”가 아니라, **Apache Software Foundation 방식으로 운영되는 프로젝트**라는 점입니다. 그래서 maintainer, PMC, vote, lazy consensus 같은 개념이 자연스럽게 등장합니다.

또 보안팀 관련 설명이 길게 들어가 있는데, 이는 Airflow가 보안 이슈를 평범한 공개 이슈처럼 다루지 않고 **작은 신뢰 집단 + 비공개 논의 + 릴리스 타이밍 관리** 방식으로 다룬다는 뜻입니다.

---

## 4. 커뮤니케이션 방식과 기대치

### `02_how_to_communicate.rst`

이 문서는 “어디서 어떤 이야기를 해야 하는가”를 설명합니다.

주요 채널:

- **dev mailing list**: 가장 공식적인 논의 공간
- **CWiki**: AIP나 큰 제안 정리용
- **GitHub PR**: 구현 세부사항 논의
- **GitHub Issues**: 버그/기능/피드백
- **Slack**: 빠른 질문, 신규 기여자 지원, contributors 채널, 문서 채널 등

핵심 원칙은 아래와 같습니다.

1. **중요한 논의는 devlist에 남아야 한다**
   - Apache 프로젝트 특성상 “devlist에 없으면 공식적으로 없던 일”에 가깝습니다.
2. **PR은 구현 상세 논의용**
   - 구조나 큰 방향 토론은 devlist가 더 맞습니다.
3. **Slack은 빠르지만 비공식 채널**
   - 빠른 길찾기에는 좋지만, 기록성과 합의의 기준은 devlist 쪽입니다.
4. **답변은 비동기적이다**
   - maintainers도 자원봉사자이므로 즉답을 기대하면 안 됩니다.
5. **PR을 끝까지 밀고 가는 책임은 작성자에게 있다**
   - 리뷰 요청, follow-up, 리마인드, 대화 수습까지 작성자 몫입니다.

문서 후반부는 커뮤니티 분위기까지 설명합니다.

- disagreement는 자연스럽다
- maintainers끼리도 의견이 다를 수 있다
- 개인 공격이 아니라 주제 중심으로 이야기해야 한다
- “Disagree but engage”가 중요하다

즉 이 문서는 기술보다 먼저 **Airflow 안에서 사람들과 일하는 방식**을 배우게 합니다.

---

## 5. 새 기여자를 위한 첫 진입 경로

### `03a_contributors_quick_start_beginners.rst`

이 문서는 첫 PR을 최대한 짧은 경로로 통과시키는 데 초점을 맞춥니다.

구조는 매우 단순합니다.

- GitHub 계정과 fork 준비
- Breeze 또는 Codespaces 중 하나 선택
- 작은 변경 하나 만들기
- `prek --all-files`
- 관련 테스트 실행
- push 후 PR 열기
- 필요하면 `upstream/main` 기준으로 rebase

초보자 기준으로 중요한 포인트는:

- 로컬 개발은 **Breeze**를 기준으로 설명합니다.
- 원격 개발은 **Codespaces**를 보여줍니다.
- DAG를 직접 만져보고 싶으면 `files/dags/`에 넣어서 Breeze에 자동 마운트되게 할 수 있습니다.
- “작은 문서 수정이나 typo 수정”이 첫 PR의 기본 예시입니다.

즉 이 문서는 breadth보다 **진입 마찰 최소화**가 목적입니다.

### `03_contributors_quick_start.rst`

이건 beginner 가이드보다 훨씬 길고, 사실상 **환경 구축과 실제 개발 루프 전체**를 담고 있습니다.

포함 범위:

- Docker / Docker Compose / Colima 설치
- uv / virtualenv 준비
- fork / clone
- prek 설정
- Breeze 설치와 초기화
- DB reset, admin user 생성
- `breeze start-airflow`
- tmux / mprocs 사용
- 로컬 UI 접속
- 테스트 실행
- PR 생성과 rebase 흐름
- IDE와 원격 환경 연결 포인터

이 문서는 현 시점의 Airflow 개발 문화를 많이 보여줍니다.

- `setup_breeze`를 통해 shim 기반 Breeze 설치를 권장
- `uv`가 기본 Python 개발 프론트엔드로 자리 잡음
- `mprocs`가 기본 terminal multiplexer이고, `tmux`는 대안
- `breeze start-airflow`가 사실상 로컬 통합 개발의 표준 시작점

즉 초보자가 아니라 **지속적으로 Airflow를 개발할 사람**을 위한 실무 문서입니다.

---

## 6. 기여 정책: Airflow가 일반 OSS와 다른 점

### `04_how_to_contribute.rst`

이 문서는 Airflow의 기여 문화에서 가장 특이한 부분을 직접 말해줍니다.

핵심 정책:

- **PR 전에 이슈를 반드시 만들 필요는 없다**
- 애매하면 **Discussion** 또는 **devlist**에서 먼저 논의
- **외부 기여자에게 이슈를 assign하지 않는다**
- 같은 이슈를 여러 사람이 병렬로 작업해도 된다
- 결과적으로는 **더 나은 PR이 머지될 수 있다**

이 정책은 꽤 강하게 서술되어 있습니다. 특히 2026년 3월부터는 “assign 요청을 기대하지 말라”는 방향이 더 명확해졌습니다.

문서가 설명하는 배경은 이렇습니다.

- assign이 배지처럼 쓰인다
- 실제로 끝까지 못 가져가는 경우가 많다
- Gen-AI로 임시 시도만 하고 흐지부지되는 사례가 생겼다
- 결국 다른 사람이 작업하기 어렵게 만드는 점유 효과가 생긴다

그래서 Airflow는 “내가 먼저 잡았으니 내 이슈”보다는,
**누가 실제로 끝까지 좋은 PR을 내느냐** 쪽에 더 무게를 둡니다.

이 문서는 시작점도 제안합니다.

- `kind:bug`
- `kind:feature`
- `good first issue`
- 문서 개선

그리고 providers 영역의 operator / hook / executor 추가도 환영한다는 메시지를 줍니다.

---

## 7. PR 문서: 실질적인 품질 기준

### `05_pull_requests.rst`

이 문서는 Airflow 기여에서 가장 중요한 문서 중 하나입니다. 단순히 PR 여는 법이 아니라, **어떤 PR이 받아들여지고 어떤 PR이 걸러지는지**를 적어둔 운영 규칙 문서입니다.

### 7-1. 기본 PR 규칙

- 초반에는 **Draft PR**로 두는 것을 권장
- 테스트를 포함할 것
- 기능 변경이면 문서도 같이 바꿀 것
- 작은 PR을 선호
- rebase를 자주 할 것
- unresolved conversation을 남겨두지 말 것
- static checks / CI가 깨지면 머지되지 않음

특히 Airflow는 `Squash and Merge`를 기본으로 사용하므로, PR 내부 커밋 히스토리보다 **PR 자체의 일관성과 리뷰 용이성**을 더 중시합니다.

### 7-2. PR 품질 기준

리뷰 전 최소 기준은 다음입니다.

1. **제목이 구체적이어야 함**
2. **본문이 비어 있으면 안 됨**
3. **정적 체크 통과**
4. **Gen-AI 사용 시 disclosure 필수**
5. **관련 없는 변경을 한 PR에 섞지 말 것**

이 기준을 못 맞추면:

- Draft로 전환되거나
- 반복 위반 시 close되거나
- 악의적 변경이 의심되면 contributor의 다른 PR까지 닫힐 수 있습니다.

### 7-3. commit identity

문서 앞부분은 commit author 정보도 강조합니다.

- 머지된 뒤 author/co-author 정보는 영구 공개
- GitHub noreply 메일을 미리 써야 익명화 가능
- co-author 항목은 GitHub 자동 익명화 대상이 아님

즉 기여자는 코드뿐 아니라 **공개될 identity 정보까지 의식해야 한다**는 뜻입니다.

### 7-4. Gen-AI 정책

이 문서에서 특히 두드러지는 부분입니다.

- Gen-AI 자체는 허용
- 하지만 **반드시 disclosure**
- 생성된 코드를 맹신하면 안 됨
- 모든 관련 정적 체크와 테스트를 직접 돌려야 함
- 관련 없는 변경이 섞였는지 사람이 직접 정리해야 함
- 품질 낮은 AI PR을 반복하면 maintainers가 review 없이 닫을 수 있음

즉 Airflow는 AI-assisted contribution을 허용하지만, **책임은 전적으로 contributor 본인에게 남긴다**는 철학입니다.

### 7-5. Airflow-specific coding standards

문서 후반은 단순 PR 지침을 넘어서 Airflow식 코딩 규칙까지 설명합니다.

- production code에서 `assert` 쓰지 않기
- `session` 파라미터를 받은 함수가 내부 commit 하지 않기
- duration 계산에는 `time.monotonic()` / `time.perf_counter()` / `Stats.timer()` 활용
- Operator `template_fields` 관련 값은 `__init__`에서 과한 검증을 하지 않기
- 새 코드에서 `AirflowException` 직접 raise를 늘리지 말고 더 구체적 예외를 쓰기

즉 이 문서는 PR 정책 문서이면서 동시에 **Airflow 코드 리뷰 룰북** 역할도 합니다.

---

## 8. 전체 기여 흐름

### `18_contribution_workflow.rst`

이 문서는 “Airflow 기여는 보통 어떤 순서로 끝나는가”를 한 장짜리 흐름으로 보여줍니다.

일반 흐름:

1. `apache/airflow` fork
2. 로컬/원격 개발 환경 준비
3. devlist / Slack / 이슈 채널 연결
4. 수정 + 테스트 + PR 생성
5. 리뷰 대응 + follow-up

이 문서가 특히 좋은 점은 **PR 작성자의 책임 범위**를 분명히 적는다는 점입니다.

- fork main을 최신으로 유지
- `upstream/main` 기준 브랜치 생성
- 구현 파일과 테스트 파일을 같이 찾기
- static checks와 tests를 먼저 통과시키기
- 필요하면 newsfragment도 추가하기
- 리뷰가 늦으면 follow-up하기

리뷰 단계 설명도 꽤 현실적입니다.

- 일반 코멘트
- line conversation
- request changes
- approval

그리고 최종 merge 조건을 비교적 명확히 정리합니다.

- tests / static checks green
- conversations resolved
- 최소 maintainer approval
- unresolved request changes 없음

즉 이 문서는 beginner guide보다 덜 설치지향적이고, **실제 PR lifecycle 중심**입니다.

---

## 9. 개발 환경 전략

### `06_development_environments.rst`

Airflow는 세 가지 환경을 나란히 둡니다.

1. **Local virtualenv**
2. **Breeze**
3. **Remote dev environments** (Codespaces, GitPod)

핵심 비교 포인트는 다음과 같습니다.

- unit test만 빠르게 돌릴 건가
- integration / CI 재현이 필요한가
- IDE 디버깅이 중요한가
- 팀과 재현성을 맞추는 게 중요한가
- 디스크와 CPU 자원을 얼마나 쓸 수 있는가

정리하면:

- **local virtualenv**: 빠르고 IDE 친화적이지만 외부 integration에는 약함
- **Breeze**: CI와 가장 가까운 재현 환경
- **remote**: 로컬 의존성이 적지만 플랫폼 제약 존재

Airflow 문서의 뉘앙스는 “세 가지 중 하나만 고르라”가 아니라, **상황 따라 복수 환경을 병행하라**에 가깝습니다.

---

## 10. 로컬 Python 개발 환경과 `uv`

### `07_local_virtualenv.rst`

이 문서는 Airflow의 현재 Python 개발 기반이 `uv` 중심이라는 점을 분명하게 보여줍니다.

핵심 내용:

- Python 버전 관리도 `uv`
- virtualenv 생성도 `uv`
- dependency sync도 `uv`
- monorepo workspace도 `uv`
- `uv.lock` 기반 재현성 유지

주요 흐름:

- `uv python install`
- `uv venv`
- `uv sync`
- package별 sync (`--package`)
- 전체 workspace sync (`--all-packages`)

provider 작업도 별도 설명이 있습니다.

- 특정 provider 디렉터리에서 `uv sync`
- root `.venv`를 공유하면서 provider용 dependency만 맞춤 설치

또 이 문서는 `uv.lock` 운용 철학도 설명합니다.

- committed lock file 사용
- `--frozen` sync 가능
- `exclude-newer = "4 days"` 쿨다운으로 최신 패키지 충격 완화
- constraint 파일도 결국 `uv.lock`에서 파생됨

즉 Airflow는 이제 “pip로 대충 editable install”보다, **workspace-aware uv monorepo 개발**을 기본 모델로 삼는다고 봐야 합니다.

---

## 11. 정적 체크 체계

### `08_static_code_checks.rst`

Airflow의 로컬 품질 게이트는 사실상 `prek` 중심입니다.

핵심 메시지:

- CI 전에 로컬에서 돌려라
- staged files 위주라 빠르다
- CI와 거의 같은 검사 체계를 로컬에 끌어온다

문서가 다루는 내용:

- `uv tool install prek` / `pipx install prek`
- `prek install`
- `prek install --hook-type pre-push`
- `prek --all-files`
- `prek --from-ref main`
- hook별 직접 실행
- `SKIP` 환경변수로 일부 hook 비활성화
- breeze 이미지 기반 hook과 local hook 차이

특히 mypy 쪽 설명이 자세합니다.

- non-provider 프로젝트는 local `uv` 기반 mypy
- providers는 여전히 breeze 기반 mypy
- hook별 `.build/mypy-venvs/`, `.build/mypy-caches/` 사용

즉 이 문서는 단순 linter 안내가 아니라, **Airflow 품질 게이트의 로컬 운영 매뉴얼**입니다.

---

## 12. Git과 브랜치 운영 방식

### `10_working_with_git.rst`

이 문서는 Airflow Git 사용법의 표준 문서입니다.

핵심 규칙:

- `upstream` = `apache/airflow`
- `origin` = 내 fork
- PR branch는 무조건 `origin`으로 push
- rebase 중심 workflow

또 branch 정책도 들어 있습니다.

- 새 개발은 기본적으로 `main`
- 특정 유지보수 라인(`v2-10-test` 등)은 별도 목적
- Helm chart는 core와 다른 branch cadence를 가짐

이 문서에서 특히 실용적인 부분은:

- fork sync 방법
- `dev/sync_fork.sh`
- `git merge-base`
- 수동 rebase 절차
- `uv.lock` conflict 해결법

즉 Airflow에서는 “merge commit 남기지 말고, 최신 main 위에 깔끔하게 rebase해 올리는 것”이 기본 전제입니다.

---

## 13. 테스트 체계 전체 지도

### `09_testing.rst`

이 문서는 테스트 종류를 한 장짜리 index처럼 보여줍니다.

Airflow 테스트는 크게 아래로 나뉩니다.

- unit tests
- integration tests
- docker compose tests
- k8s tests
- helm unit tests
- system tests
- task sdk integration tests
- airflow ctl integration tests
- python client tests
- package distribution tests
- dag testing

즉 Airflow에서 “테스트 돌렸다”는 말은 단일 pytest 실행이 아니라, **어느 층위의 테스트를 돌렸는가**까지 같이 봐야 합니다.

---

## 14. `testing/` 하위 문서 정리

참고로 `testing/README.rst` 자체는 독립적인 설명 문서라기보다, `09_testing.rst`로 되돌아가게 하는 짧은 인덱스 역할을 합니다.

### `testing/unit_tests.rst`

이 문서는 가장 깊고 실무적인 테스트 문서입니다.

주요 규칙:

- pytest 사용
- mock / parametrize 적극 사용
- warning은 `pytest.warns`로 다룰 것
- 시간 관련 테스트는 `time-machine`
- DB test와 non-DB test를 엄격히 구분
- DB 접근 테스트는 `@pytest.mark.db_test`

또 Airflow test type 분류도 설명합니다.

- `API`, `CLI`, `Core`, `Operators`, `WWW`, `Providers`, `Other`
- `All-Postgres`, `All-MySQL`, `All-Quarantined`, `All`

non-DB test는 xdist 병렬화가 가능하지만, DB test는 공유 상태 때문에 그렇게 못 하고 test-type 단위 병렬화를 사용합니다.

이 문서가 반복해서 말하는 철학은:

- 가능한 한 non-DB unit test를 많이 만들라
- DB test가 필요하다면 정확히 mark 하라
- pytest collection 단계에서 DB 접근이 터지지 않도록 top-level object 생성도 조심하라

### `testing/integration_tests.rst`

외부 서비스와 붙는 integration tests 문서입니다.

- local virtualenv에서는 못 돌리고 **Breeze 전용**
- integration은 명시적으로 enable 해야 함
- 예: cassandra, celery, kafka, kerberos, keycloak, mongo, redis, localstack 등
- `pytest --integration mongo tests/integration`
- `breeze testing core-integration-tests`
- `breeze testing providers-integration-tests`

후반부는 새 integration을 추가하는 절차도 설명합니다.

- `scripts/ci/docker-compose/integration-<name>.yml`
- host port 상수 등록
- shell params 설정
- health check 추가
- pytest marker 작성

즉 이 문서는 **integration 소비자용 + integration 추가 작성자용** 문서를 겸합니다.

### `testing/docker_compose_tests.rst`

문서에 공개된 Docker Compose quick start가 실제로 동작하는지 검증합니다.

흐름:

1. prod image build
2. 문서용 compose file로 배포
3. example DAG trigger 확인
4. 실패 시 logs dump
5. 필요하면 compose 유지 후 수동 디버깅

즉 “문서의 quick start가 진짜 깨지지 않았는지”를 보는 배포 검증 테스트입니다.

### `testing/k8s_tests.rst`

Kubernetes 테스트 문서입니다. 사실상 별도 운영 가이드에 가깝습니다.

핵심:

- Kind 기반 cluster 관리
- `breeze k8s setup-env`
- multi-cluster 지원
- `create-cluster`, `configure-cluster`, `build-k8s-image`, `upload-k8s-image`, `deploy-airflow`, `tests`, `shell`, `k9s`
- skaffold 기반 dev hot reload

즉 단순 테스트 문서가 아니라, **Airflow를 로컬 Kind cluster에 올려서 K8s 관점으로 개발/디버깅하는 방법**까지 포함합니다.

### `testing/helm_unit_tests.rst`

Helm chart 테스트는 Pythonic 방식으로 유지합니다.

- chart render 결과를 Python dict로 받아서 검사
- `jmespath` 사용
- `breeze testing helm-tests`
- 패키지 단위 test type 선택 가능
- 병렬 pytest 사용 가능

Helm 쪽을 shell script나 snapshot만으로 보지 않고, **Python test infra 안에 통합**해 놓은 점이 특징입니다.

### `testing/system_tests.rst`

system test는 provider/operator를 실제 DAG처럼 돌려보는 테스트입니다.

목적:

- operator happy path 검증
- provider 회귀 방지
- example DAG 제공
- 문서 snippet 원천 역할

실행 방식:

- Airflow DAG로 직접 올려 실행
- Breeze + pytest
- 실제 executor를 쓰거나 generic executor 동작을 시뮬레이션 가능
- 외부 credential forwarding도 지원

즉 system tests는 테스트, 예제, 문서 자산이 동시에 되는 구조입니다.

### `testing/task_sdk_integration_tests.rst`

Task SDK와 Airflow execution API 서버 간의 계약을 검증합니다.

- prod image 먼저 build
- local mount 기반 빠른 iteration 가능
- 필요하면 CI처럼 local mount 없이도 재현 가능
- Breeze 또는 `uv run pytest`
- Docker Compose logs 기반 디버깅 절차 포함

Task SDK가 별도 distribution이기 때문에 필요한 **cross-package compatibility test** 문서입니다.

### `testing/airflow_ctl_tests.rst`

`airflowctl` CLI의 통합 테스트 문서입니다.

- full Airflow environment를 띄운 뒤 검증
- Breeze 기반
- scheduled CI + local run 가능

### `testing/python_client_tests.rst`

Python API client용 테스트 문서입니다.

- `airflow-client-python` repo clone
- client distribution build
- Airflow webserver against client test

즉 본 저장소만이 아니라 **별도 client repo와 연동되는 테스트 흐름**까지 다룹니다.

### `testing/testing_packages.rst`

릴리스 후보 distribution을 수동 검증하는 문서입니다.

- `dist/` 폴더 사용
- source build와 PyPI release candidate를 섞어서 검증 가능
- `--mount-sources remove`
- `--use-distributions-from-dist`
- provider 조합 테스트 가능

release manager, RC 검증자, packaging 관련 contributor에게 중요한 문서입니다.

### `testing/dag_testing.rst`

가볍게 DAG만 빠르게 시험하는 문서입니다.

- `dag.test()`
- `if __name__ == "__main__":`
- `airflow dags test`

즉 전체 Airflow test suite와 별개로, **DAG authoring / 디버깅 속도를 높이는 실용 팁**에 가깝습니다.

### `testing/airflow_e2e_tests.rst`

Airflow 전체 시스템을 prod image 기반으로 실제 실행하는 E2E 테스트입니다.

- prod image build
- `airflow-e2e-tests` pytest 실행
- `breeze testing airflow-e2e-tests`
- CI workflow와 artifact 위치 설명

이건 unit/integration보다 한 단계 위에서 **전체 배포 단위 동작**을 보는 테스트입니다.

---

## 15. 문서 빌드 체계

### `11_documentation_building.rst`

Airflow 3 이후 문서 구조는 distribution별로 쪼개졌습니다.

예:

- `airflow-core/docs`
- `providers/**/docs`
- `chart/docs`
- `task-sdk/docs`
- `docker-stack-docs`
- `providers-summary-docs`

핵심 명령은 `build-docs`입니다.

- local venv에서는 `uv run --group docs build-docs`
- iterating에는 `--autobuild`
- 여러 distribution을 동시에 build 가능
- 전체 repo docs도 build 가능
- local이 안 되면 `breeze build-docs` fallback 가능

문서에서 특히 실용적인 부분:

- Python 3.11 고정 권장
- `enchant` C library 이슈
- intersphinx inventory cache
- `document isn't included in any toctree` 같은 Sphinx 특유의 오류 설명
- include path를 distribution 경계를 의식해서 잡는 방법

즉 Airflow docs는 그냥 Markdown 수정이 아니라, **multi-distribution Sphinx build system**입니다.

---

## 16. Providers와 배포 구조

### `12_provider_distributions.rst`

이 문서는 provider 기여자를 위한 핵심 문서입니다.

핵심 구조:

- `apache-airflow`
- `apache-airflow-core`
- `apache-airflow-task-sdk`
- `apache-airflow-providers-*`

provider는 대체로 아래 구조를 가집니다.

- `pyproject.toml`
- `provider.yaml`
- `src/airflow/providers/...`
- `docs`
- `tests/unit`
- `tests/integration`
- `tests/system`

`provider.yaml`은 특히 중요합니다.

- provider 메타데이터
- integration 목록
- operator / hook / sensor / transfer 목록
- connection types, extra links, auth backends 등

즉 provider 기여는 소스코드만이 아니라 **메타데이터와 문서 자동생성 체계**까지 같이 만지게 됩니다.

또 이 문서는 provider naming convention도 설명합니다.

- namespace package 구조
- module naming
- operator / hook / sensor naming
- transfer operator naming
- docs / example dag / system test 요구사항

후반부는 provider breaking changes와 dependency minimum version bump에 대한 태도도 설명합니다.

- provider는 core보다 breaking change를 조금 더 유연하게 다룸
- 하지만 changelog와 migration path를 명확히 적어야 함
- 최종 major/breaking 판정은 release manager의 판단을 많이 받음

---

## 17. 의존성 관리와 constraint 철학

### `13_airflow_dependencies_and_extras.rst`

이 문서는 Airflow 의존성 관리 문서입니다. 단순히 “requirements 어디에 적냐” 수준이 아닙니다.

핵심 내용:

- 모든 distribution은 `pyproject.toml` 기반
- `uv workspace`로 monorepo 전체를 묶음
- generated dependency block은 수동 편집 금지
- upper-bound는 드물게만
- lower-bound는 신중히
- contributor가 distribution version을 임의로 올리면 안 됨
- cross-distribution feature 의존이 생기면 `# use next version` 규칙 사용

또 이 문서는 Airflow가 왜 constraints 파일을 유지하는지도 설명합니다.

Airflow는:

- application처럼 reproducible install이 필요하고
- library처럼 열린 의존성도 필요합니다.

이 모순을 constraints 파일로 풀고 있습니다.

- `constraints`
- `constraints-source-providers`
- `constraints-no-providers`

결국 `uv.lock`과 constraints 파일이 함께 Airflow의 설치 재현성을 담당합니다.

---

## 18. DB migration과 메타데이터 스키마 변경

### `14_metadata_database_updates.rst`

DB migration이 필요한 경우 따라야 하는 문서입니다.

핵심:

- Alembic 기반 migration 생성
- `breeze generate-migration-file -m ...`
- 또는 alembic 직접 사용
- prek가 migration file naming 규칙을 자동 수정
- migration script에서 ORM class 직접 참조하지 말 것

rebase conflict 처리도 안내합니다.

- migration file prefix 충돌
- `docs/.../migrations-ref.rst` 충돌
- `prek update-migration-references --all-files`

또 Airflow migration 과정에 외부 application migration을 hooking하는 방법까지 설명합니다.

즉 이 문서는 단순 schema migration 추가가 아니라 **Airflow migration 시스템에 안전하게 들어가는 법**을 다룹니다.

---

## 19. UI / Node / 프론트엔드 기여

### `15_node_environment_setup.rst`

UI 쪽 contributor에게 필요한 문서입니다.

핵심 기술 스택:

- React
- pnpm
- Vite
- Chakra UI
- React Query

기본 요구:

- recent Node / pnpm
- 메모리 부족 시 `NODE_OPTIONS`
- `breeze start-airflow --dev-mode`로 hot reload

명령 수준으로는:

- `pnpm install`
- `pnpm dev`
- `pnpm build`
- `pnpm format`
- `pnpm lint`
- `pnpm test`
- `pnpm codegen`

또 문서 후반에는 UI best practices도 있습니다.

- Chakra semantic tokens 우선
- `useEffect` 남용 금지
- local state / URL state / localStorage / server state 역할 구분
- new component는 테스트 포함
- `openapi-gen/` 같은 generated code는 직접 수정하지 않기

즉 이 문서는 frontend contributor를 위한 별도 스타일 가이드 역할을 합니다.

---

## 20. API, execution API, 아키텍처 관련 심화 문서

### `16_adding_api_endpoints.rst`

FastAPI endpoint 추가 절차 문서입니다.

- `public` endpoint vs `ui` endpoint 구분
- route 등록
- permission / params / return type 설정
- extensive tests 추가
- `/docs`에서 generated docs 확인
- OpenAPI spec generated file 갱신

즉 endpoint는 단순 route 함수 하나가 아니라, **backward compatibility와 generated spec**까지 고려해야 합니다.

### `19_execution_api_versioning.rst`

Task Execution API versioning 문서입니다.

- Cadwyn 사용
- CalVer 기반 version module (`vYYYY_MM_DD`)
- schema / behavior 변경은 version migration 필요
- old/new version tests 함께 유지
- backward compatibility 우선

이 문서는 Task SDK와 Airflow core 사이의 contract가 점점 더 중요해지는 흐름을 보여줍니다.

### `17_architecture_diagrams.rst`

다이어그램도 코드로 관리합니다.

- `diagram_*.py`
- graphviz 필요
- prek hook `generate-airflow-diagrams`
- source + generated png + `.md5sum` 커밋

즉 아키텍처 그림도 “사람 손으로 그린 이미지”보다 **재생성 가능한 artifact**로 보려는 철학입니다.

---

## 21. 디버깅과 로컬 통합 개발 보조 문서

### `20_debugging_airflow_components.rst`

Breeze에서 Airflow component를 원격 디버깅하는 법을 설명합니다.

- `breeze start-airflow --debug scheduler`
- component별 debug port
- `debugpy` / `pydevd-pycharm`
- VS Code attach 설정
- `mprocs`와 디버깅 같이 쓰기

즉 scheduler, triggerer, dag-processor, api-server 같은 프로세스를 **IDE에서 붙어서 보는 방법**을 제공합니다.

### `21_keycloak_client_settings.rst`

상당히 niche하지만, Breeze에서 Keycloak integration을 띄웠을 때 Airflow용 Keycloak client를 어떻게 설정하는지 정리해 둡니다.

이 문서는 범용 contributor보다 **SSO / auth 통합 실험을 하는 사람**에게 의미가 있습니다.

### `mprocs/MPROCS_QUICK_REFERENCE.md`

`mprocs`를 Breeze의 기본 multiplexer로 쓰는 방법을 요약합니다.

- `breeze start-airflow`
- `--terminal-multiplexer tmux`
- `breeze setup config --terminal-multiplexer ...`
- 키보드 단축키
- 어떤 컴포넌트가 관리되는지

즉 `mprocs`는 단순 도구가 아니라, Airflow component 여러 개를 한 화면에서 다루는 기본 UX 일부가 되었습니다.

### `mprocs/mprocs.yaml`

이건 설명 문서라기보다 예시 설정입니다.

- scheduler
- api_server
- triggerer
- dag_processor
- shell
- optional celery_worker / flower

직접 수정용 template라기보다 “mprocs가 어떤 프로세스 셋을 띄우는지” 보여주는 샘플에 가깝습니다.

---

## 22. IDE / 원격 개발 문서군

### `quick-start-ide/README.rst`

IDE 관련 하위 문서 인덱스입니다.

### `contributors_quick_start_pycharm_intellij.rst`

PyCharm / IntelliJ용 설정 문서입니다.

중요 포인트:

- `uv run dev/ide_setup/setup_idea.py`
- single-module / multi-module 모드
- IntelliJ Ultimate가 multi-module에 적합
- provider, task-sdk, devel-common source/test roots를 정확히 잡아야 함
- DAG 디버깅과 branch 생성까지 IDE UI 기준으로 설명

Airflow 3의 multi-distribution 구조 때문에 IDE 설정 난이도가 올라갔고, 그걸 스크립트로 최대한 자동화하려는 의도가 보입니다.

### `contributors_quick_start_vscode.rst`

VS Code용 문서입니다.

- provider tests path를 Pylance extra paths에 넣어야 할 수 있음
- `dag.test()` 기반 local debug 흐름
- `launch.json` 구성 예시
- branch 생성 UI 안내

### `contributors_quick_start_codespaces.rst`

GitHub Codespaces 안내 문서입니다.

- fork 후 codespace 생성
- 터미널이 Breeze 환경일 수 있음
- Docker 미동작 시 `docker info`, socket, user group 확인
- 안 되면 devcontainer rebuild

### `contributors_quick_start_gitpod.rst`

GitPod 경로 문서입니다.

- fork 후 Gitpod URL 구성
- `setup_breeze`
- DB reset / admin user 생성
- `breeze start-airflow` / `--dev-mode`

이 하위 문서들은 “환경 비교”보다, **특정 IDE/플랫폼에서 Airflow 개발이 실제로 돌아가게 만드는 보조 설명**에 가깝습니다.

---

## 23. provider / schema / best practice 관련 특수 문서

### `23_provider_hook_migration_to_yaml.rst`

기존 hook code에 있던 connection form metadata를 `provider.yaml` 선언형 방식으로 옮기는 문서입니다.

배경:

- 예전에는 hook가 `get_connection_form_widgets()` 같은 메서드로 UI metadata를 제공했음
- 이 방식은 `wtforms`, `flask_appbuilder` 같은 무거운 dependency를 API server가 들고 와야 함
- YAML 선언형 방식은 import cost와 startup cost를 줄임

문서는 `ui-field-behaviour`, `conn-fields`, migration helper script 사용법을 예시와 함께 보여줍니다.

### `24_proposing_best_practices_and_air_rules.rst`

새로운 Airflow best practice나 Ruff `AIR` rule을 제안하는 절차입니다.

- 먼저 lazy consensus 또는 vote
- 그 다음 문서화
- 필요하면 Ruff rule 개발 병행
- merge 후 docs 정리

짧지만 “Airflow 스타일 규칙도 커뮤니티 합의 대상”이라는 점을 잘 보여줍니다.

---

## 24. maintainer 운영 문서

### `25_maintainer_pr_triage.md`

이 문서는 contributor용보다는 maintainer workflow 설명서에 가깝습니다.

핵심 내용:

- Claude Code 기반 `pr-triage` skill 사용
- first-pass triage 자동화
- deterministic rule 기반 PR 분류
- draft / comment / close / rerun / mark-ready / ping / approve-workflow 등의 액션
- contributor-facing AI comment에는 attribution footer 포함

중요한 구조는 **2-stage review**입니다.

1. Stage 1: 자동화된 triage
2. Stage 2: human maintainer review

즉 Airflow는 PR 수가 많아서, maintainers가 기계적으로 소모되는 일을 줄이기 위해 **review 파이프라인 자체를 문서화하고 자동화**하고 있습니다.

---

## 25. 이 subtree를 다 읽고 나서 보이는 Airflow 기여 철학

이 문서 묶음을 전체로 보면 Airflow는 대략 이런 프로젝트로 읽힙니다.

### 25-1. 작은 PR과 강한 로컬 검증

Airflow는 contributor에게 아래를 강하게 기대합니다.

- `prek`
- 관련 테스트
- 문서 수정
- rebase
- review thread 정리

즉 “코드가 돌아간다”보다 **리뷰 가능한 형태로 정리되어 있느냐**가 매우 중요합니다.

### 25-2. assign 기반 점유 문화가 아니다

일반적인 GitHub OSS와 달리:

- 이슈 assign을 거의 안 함
- 같은 이슈 병렬 작업 허용
- 더 나은 PR이 이김

이건 Airflow가 최근 AI 시대의 contributor 흐름까지 반영해 의도적으로 만든 문화입니다.

### 25-3. `uv` + monorepo + multi-distribution이 현재 기본 토대

문서 전반에서 보이는 현재 기본값은:

- `uv sync`
- `uv.lock`
- `setup_breeze`
- package별 `pyproject.toml`
- provider와 core의 분리

즉 최신 Airflow 기여는 옛날식 단일 패키지 editable install 감각으로 보면 자꾸 어긋납니다.

### 25-4. Breeze는 단순 개발 도구가 아니라 로컬 CI 재현 플랫폼

Airflow 문서에서 Breeze는 편의도구가 아니라,

- integration test용 환경
- DB/backend matrix 재현 환경
- UI/API/K8s/Helm/Docker Compose까지 이어지는 재현 기반

으로 취급됩니다.

### 25-5. 테스트가 매우 다층적이다

Airflow에서 테스트는 하나가 아닙니다.

- pure unit
- DB unit
- integration
- system
- Docker Compose
- Helm
- K8s
- E2E
- Task SDK
- airflowctl
- Python client

즉 기여자가 어느 층을 건드렸는지에 따라 필요한 테스트 종류가 크게 달라집니다.

### 25-6. docs / provider metadata / packaging도 1급 시민이다

Airflow는 코드만 수정하는 프로젝트가 아닙니다.

- docs build가 distribution별로 존재
- provider.yaml이 중요
- generated metadata가 많음
- release candidate distribution test 문서가 별도로 존재

즉 provider나 docs 영역도 core code 못지않게 **운영 체계에 깊게 연결**되어 있습니다.

---

## 26. 실제로 읽을 때 추천 순서

처음 읽는 사람 기준으로는 아래 순서를 추천합니다.

### A. 첫 PR만 빨리 보내고 싶을 때

1. `README.rst`
2. `03a_contributors_quick_start_beginners.rst`
3. `04_how_to_contribute.rst`
4. `05_pull_requests.rst`
5. `10_working_with_git.rst`
6. `09_testing.rst`

### B. 실제로 한동안 Airflow를 계속 만질 때

1. `06_development_environments.rst`
2. `07_local_virtualenv.rst`
3. `08_static_code_checks.rst`
4. `18_contribution_workflow.rst`
5. `12_provider_distributions.rst`
6. `13_airflow_dependencies_and_extras.rst`

### C. 특정 분야 기여자용

- UI 기여자: `15_node_environment_setup.rst`
- API 기여자: `16_adding_api_endpoints.rst`, `19_execution_api_versioning.rst`
- DB 변경: `14_metadata_database_updates.rst`
- provider 기여자: `12_provider_distributions.rst`, `23_provider_hook_migration_to_yaml.rst`
- 테스트 인프라/배포 기여자: `testing/*`, `11_documentation_building.rst`, `k8s_tests.rst`, `docker_compose_tests.rst`

---

## 27. 마지막 메모

이 subtree는 단순 onboarding 문서라기보다, **Airflow라는 대형 Apache 프로젝트가 스스로를 운영하는 방법을 문서화한 저장소 내부 handbook**에 가깝습니다.

읽고 나면 분명해지는 점은 아래입니다.

- Airflow는 contributor 수가 많고 표면적 범위가 넓다.
- 그래서 “사람의 선의”만으로 굴리는 게 아니라, 문서/도구/정적 체크/테스트/triage 프로세스로 질서를 만든다.
- 이 기여 문서 세트는 그 질서를 새 기여자에게 주입하는 역할을 한다.

짧게 말하면, Airflow에 기여한다는 건 단순히 코드 몇 줄을 바꾸는 일이라기보다 **Airflow가 이미 만들어 둔 개발 운영 체계를 이해하고 그 흐름에 맞춰 일하는 과정**에 더 가깝습니다.
