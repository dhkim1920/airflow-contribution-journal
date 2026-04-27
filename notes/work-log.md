# PR 열기 전후 체크리스트

Airflow에서는 단순히 코드가 돌아가는 것만으로는 부족합니다. PR 제목/설명, Draft 사용, 대화 해결, 리베이스, 테스트, 정적 체크까지 모두 품질 기준에 들어갑니다.

## 작업 브랜치 만들기

포크의 `main`을 최신으로 맞춘 다음 브랜치를 만드는 것이 가장 안전합니다.

```bash
git fetch upstream
git checkout main
git rebase upstream/main
git checkout -b <topic-branch>
```

## 수정 후 바로 확인할 것

- 변경 범위가 한 가지 주제로 묶여 있는가
- 테스트를 같이 넣었는가
- 기능 변경이면 문서도 같이 바꿨는가
- 새 파일이 있으면 라이선스 헤더가 필요한가
- 커밋 메시지가 맥락을 설명하는가

## PR 올리기 전 로컬 체크

```bash
prek --all-files
```

가능하면 관련 테스트도 먼저 돌립니다.

```bash
breeze testing core-tests
```

문서에서 강조하는 핵심은 간단합니다. **리뷰 전에 로컬에서 최대한 문제를 제거하라**는 것입니다.

## PR은 Draft로 시작하는 편이 좋다

Airflow 공식 PR 가이드 기준으로는, 품질 체크와 테스트가 충분히 정리되기 전이거나 방향 피드백이 먼저 필요할 때 **Draft PR** 상태가 잘 맞습니다.

특히 아래 중 하나면 Draft가 자연스럽습니다.

- 아직 테스트가 덜 됨
- 방향 피드백이 먼저 필요함
- CI가 아직 빨갛고 원인 파악 중임

## PR 본문에 꼭 들어가야 할 것

- 무엇을 바꾸는지
- 왜 바꾸는지
- 테스트를 어떻게 했는지
- 필요한 문서 변경이 있는지
- 생성형 AI 도움을 받았다면 그 사실 공개

공식 가이드는 **제목이 너무 일반적이거나, 본문이 비어 있거나, 설명이 부실하면** 품질 미달로 판단할 수 있다고 말합니다.

## 리뷰 도중의 기본 태도

- 코멘트가 달리면 가능한 빨리 맥락을 설명한다.
- 무조건 다 수용할 필요는 없지만, 근거를 갖고 응답한다.
- unresolved conversation이 남아 있으면 머지 준비가 덜 된 것으로 보는 편이 안전합니다.
- 오래 열린 PR일수록 자주 리베이스해서 충돌을 줄인다.

## 리베이스 기본 흐름

Airflow는 rebase 중심 프로젝트입니다.

```bash
git fetch upstream
git checkout <topic-branch>
git rebase upstream/main
git push --force-with-lease
```

공식 문서도 `git push --force-with-lease`를 기준으로 설명합니다. 혼자 작업 중인 포크 브랜치라면 일반적인 PR 유지 방식입니다.

## 머지되기 쉬운 PR의 조건

- static checks 통과
- 관련 테스트 통과
- 대화(conversation) 해결
- maintainer review/approval 확보
- 범위가 작고 일관됨

또한 maintainers는 보통 **Squash and Merge**를 사용하므로, PR 내부 커밋 수보다 PR 자체의 일관성과 설명력이 더 중요합니다.

## 제출 직전 체크리스트

```md
- [ ] PR 제목이 구체적이다
- [ ] PR 본문에 what / why / test가 있다
- [ ] 관련 테스트를 로컬에서 돌렸다
- [ ] 문서 변경이 필요하면 같이 반영했다
- [ ] unrelated change를 제거했다
- [ ] Draft 여부를 현재 상태에 맞게 설정했다
- [ ] 필요하면 AI 사용 사실을 PR 본문에 적는다
- [ ] upstream/main 기준으로 리베이스했다
```
