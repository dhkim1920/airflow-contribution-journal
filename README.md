# Airflow 컨트리뷰션 가이드

이 저장소는 Apache Airflow에 기여할 때 필요한 흐름을 한국어로 빠르게 훑고, 바로 실행에 옮길 수 있게 정리한 가이드입니다.
핵심 목표는 **이슈 찾기 → 개발 환경 준비 → 수정 → 테스트 → PR**까지 실제 순서대로 따라가게 만드는 것입니다.

> 비공식 정리본이며 Apache Software Foundation과 무관합니다. 최종 기준은 항상 Airflow 공식 저장소와 공식 문서입니다.

## 가장 짧은 시작 경로

Airflow에 처음 PR을 보내려면 아래 순서로 가면 됩니다.

1. `apache/airflow`를 포크합니다.
2. 로컬은 보통 **Breeze**(Airflow 개발용 컨테이너 환경), 원격 개발은 **GitHub Codespaces**를 사용합니다.
3. 작은 문서 수정이나 명확한 버그 하나를 고릅니다.
4. 브랜치를 만들고 수정합니다.
5. 관련 테스트와 정적 체크를 로컬에서 돌립니다.
6. 내 포크에 푸시하고 **Draft PR**로 엽니다.
7. 리뷰 코멘트를 반영하고, 필요하면 `upstream/main` 기준으로 자주 리베이스합니다.

## Airflow 컨트리뷰션에서 먼저 알아둘 점

- PR을 보내기 전에 **반드시 이슈를 먼저 만들 필요는 없습니다.**
- 다만 버그인지 기능인지 애매하거나 범위가 큰 변경이면 GitHub Discussion이나 devlist에서 먼저 논의하는 편이 안전합니다.
- Airflow는 보통 **외부 기여자에게 이슈를 assign하지 않습니다.** 그냥 작업 후 PR을 올리면 됩니다.
- 여러 사람이 같은 문제를 동시에 풀 수도 있고, 그중 더 나은 PR이 머지될 수 있습니다.
- Git 흐름은 merge보다 **rebase 중심**입니다.
- PR은 작고 한 가지 주제에 집중될수록 리뷰가 잘 됩니다.

## 어떤 기여부터 시작하면 좋은가

처음에는 아래 우선순서를 추천합니다.

1. 문서 오타, 설명 보완, 링크 수정
2. 재현 가능한 작은 버그 수정
3. 테스트 보강
4. 범위가 작은 기능 개선

반대로 처음부터 피하는 게 좋은 것은 아래입니다.

- 구조를 크게 흔드는 리팩터링
- 한 PR에 여러 주제를 섞는 변경
- 브랜치 정책이나 릴리스 흐름 이해 없이 하는 큰 변경
- 테스트 없이 통과시키려는 수정

## 이 저장소 사용법

- `notes/issues-to-explore.md`: 어떤 이슈를 고를지 판단하는 기준
- `notes/research-log.md`: 개발 환경, 테스트, 코드 읽기 흐름 정리
- `notes/work-log.md`: 실제 PR 올릴 때 따라갈 체크리스트
- `resources/official-links.md`: 공식 문서 바로가기
- `ATTRIBUTION.md`: 출처/인용 원칙

## 빠른 문서 이동

- 이슈 고르기: [notes/issues-to-explore.md](notes/issues-to-explore.md)
- 개발 환경과 테스트: [notes/research-log.md](notes/research-log.md)
- PR 준비와 리뷰 대응: [notes/work-log.md](notes/work-log.md)
- 공식 링크: [resources/official-links.md](resources/official-links.md)

## 아주 짧은 실행 예시

```bash
git clone https://github.com/<your-id>/airflow.git
cd airflow
git remote add upstream https://github.com/apache/airflow.git
./scripts/tools/setup_breeze
git checkout -b docs/fix-small-typo
```

수정 후에는 보통 이런 흐름입니다.

```bash
prek --all-files
breeze testing core-tests
git push -u origin docs/fix-small-typo
```

테스트 명령은 변경 영역에 맞게 더 좁히는 것이 좋고, 문서-only PR이면 전체 테스트 대신 관련 문서 빌드 가이드 확인과 정적 체크 중심으로 판단해도 됩니다. 최소한 `prek --all-files`는 먼저 돌리고, 필요하면 공식 문서 빌드 가이드를 따라 현재 변경에 맞는 검증 절차를 추가로 확인하는 편이 좋습니다.

## 기준 문서

이 저장소 내용은 주로 아래 공식 문서를 바탕으로 요약했습니다.

- Airflow Contributors' Guide
- Your First Airflow Pull Request — 15-Minute Guide
- How to contribute
- Pull Requests
- Working with Git
- Contribution Workflow
- Testing
