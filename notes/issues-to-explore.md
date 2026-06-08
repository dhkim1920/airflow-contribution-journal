# 어떤 Airflow 이슈를 고르면 좋은가

Airflow는 이슈를 먼저 assign 받아야만 작업할 수 있는 프로젝트가 아닙니다. 공식 가이드 기준으로는 **이슈를 assign 받지 않아도 바로 PR을 열 수 있고**, 여러 사람이 같은 문제를 병렬로 풀 수도 있습니다. 그래서 중요한 건 “찜”이 아니라 **내가 끝까지 가져갈 수 있는 범위인지**입니다.

## 이번에 실제로 고른 이슈

- 이슈: [apache/airflow#40210](https://github.com/apache/airflow/issues/40210) - `cncf.kubernetes` operators/executors에 필요한 access privilege 문서화
- 분류: docs
- 라벨: `good first issue`, `kind:documentation`
- 선택일: 2026-06-08

이 이슈를 첫 실전 후보로 고른 이유는 범위가 문서 중심이고, Airflow maintainer가 이미 `good first issue`로 표시했으며, 구현 방향 힌트도 남아 있기 때문입니다. 특히 Helm chart의 RBAC 템플릿을 참고해 KubernetesExecutor와 KubernetesPodOperator 사용자가 필요한 권한을 이해할 수 있게 정리하는 문제라, 코드 변경보다 리뷰/문서 품질 기준을 익히기에 좋습니다.

먼저 볼 링크와 파일:

- 이슈 본문과 코멘트: <https://github.com/apache/airflow/issues/40210>
- 이전 PR: <https://github.com/apache/airflow/pull/53540>
- 참고 후보 파일: `chart/templates/rbac/pod-launcher-role.yaml`
- 참고 후보 위치: `providers/cncf/kubernetes/docs/`

주의할 점은 이전 PR [#53540](https://github.com/apache/airflow/pull/53540)이 있었지만 merge되지 않고 stale로 닫혔다는 점입니다. 방향 자체가 거절된 흔적은 없고, 코멘트상 docs/static check 실패가 남아 있던 흐름이라, 이전 PR을 그대로 베끼기보다 현재 main 기준 문서 구조와 static check를 맞춰 작게 다시 제안하는 쪽이 좋아 보입니다.

이번에 보류한 후보:

- [#68177](https://github.com/apache/airflow/issues/68177), [#68178](https://github.com/apache/airflow/issues/68178), [#68174](https://github.com/apache/airflow/issues/68174): 이미 닫는 PR이 열려 있어 첫 이슈로 부적합
- [#52516](https://github.com/apache/airflow/issues/52516): UI 설계 논의가 먼저 필요한 기능 이슈
- [#53410](https://github.com/apache/airflow/issues/53410): 최근 작업 의사를 밝힌 사람이 있고, 민감정보/UI 영향이 있어 초반 후보로는 리스크가 큼
- [#56060](https://github.com/apache/airflow/issues/56060): `Can't Reproduce`, `stale`, `pending-response` 상태라 재현부터 불확실함
- [#46293](https://github.com/apache/airflow/issues/46293): KubernetesExecutor/Task SDK 변화와 얽혀 범위가 커질 가능성이 큼

다음 액션은 Airflow 본 repository에서 현재 문서 구조를 확인하고, #53540의 접근을 참고하되 더 작은 문서 PR로 다시 작성하는 것입니다.

## 처음 기여할 때 추천하는 이슈

### 1. 문서 수정

가장 진입 장벽이 낮습니다.

- 설명이 낡았거나 불명확한 부분
- 오타, 예제 오류, 끊어진 링크
- 설치/테스트 절차에서 빠진 설명

장점은 코드베이스 전체를 깊게 몰라도 되고, PR 품질 기준과 리뷰 흐름을 익히기에 좋다는 점입니다.

### 2. 작은 버그 수정

다음 조건이면 초반에 잡기 좋습니다.

- 재현 단계가 명확함
- 수정 대상 파일과 테스트 위치를 어느 정도 추적 가능함
- 영향 범위가 좁음

Airflow 공식 문서에서도 `kind:bug`와 `good first issue` 라벨을 시작점으로 보라고 안내합니다.

### 3. 테스트 보강

실제 코드 변경보다 범위가 명확한 경우가 많습니다.

- 누락된 케이스 추가
- 회귀 버그 재현 테스트 추가
- 문서에는 설명돼 있는데 테스트가 약한 부분 보강

## 처음엔 피하는 것이 좋은 이슈

- 여러 서브시스템을 동시에 건드리는 기능 추가
- 대규모 리팩터링
- 릴리스 브랜치/백포트 정책 이해가 필요한 변경
- 토론이 길게 이어지는 설계 이슈
- 내가 재현도 못 한 버그

## 이슈를 고르기 전에 확인할 체크리스트

아래 질문 중 다수에 “예”가 나오면 시작하기 좋습니다.

- 문제 설명을 내가 이해했는가?
- 로컬에서 재현하거나, 최소한 관련 코드 위치를 찾을 수 있는가?
- 테스트를 어디에 추가해야 할지 감이 오는가?
- 한 PR 안에 끝낼 수 있는가?
- 문서 수정이 같이 필요한지 판단 가능한가?

## Airflow 쪽 커뮤니케이션 원칙

- assign 요청에 기대지 말고, 필요하면 “working on it” 정도만 남기면 충분합니다.
- 확실하지 않으면 Issue보다 **Discussion**이나 **devlist**가 더 맞을 수 있습니다.
- 이미 다른 PR이 있어도, 더 나은 접근이 있으면 별도 PR을 올릴 수 있습니다.

## 실전 선택 순서

1. `good first issue` 또는 문서 수정부터 본다.
2. `kind:bug`, `kind:feature` 라벨을 보되 범위가 작은 것만 고른다.
3. 관련 파일과 테스트 파일을 먼저 찾는다.
4. 로컬에서 재현/수정 가능성이 보이면 시작한다.
5. 너무 커 보이면 더 작은 문서/테스트 기여로 내려간다.

## 작업 시작 전 내 메모 템플릿

```md
- 이슈/PR 링크:
- 분류: docs / bug / feature / test
- 왜 지금 하기 좋은가:
- 수정할 것 같은 파일:
- 같이 볼 테스트 파일:
- 로컬에서 먼저 확인할 명령:
- PR 범위를 작게 유지하기 위한 기준:
```
