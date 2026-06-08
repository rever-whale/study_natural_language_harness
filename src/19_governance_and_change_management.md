# 19. 팀 거버넌스와 변경 관리

자연어 하네스는 팀 공용 지식베이스다. 따라서 개인 노트처럼 관리하면 오래가지 않는다. 누가 바꿀 수 있는지, 어떤 변경은 리뷰가 필요한지, 언제 삭제해야 하는지 정해야 한다.

거버넌스의 목적은 문서를 무겁게 만드는 것이 아니다. AI가 참조하는 지식이 팀의 실제 합의와 계속 일치하도록 만드는 것이다.

## 문서 등급

모든 문서를 같은 무게로 다루지 않는다.

| 등급 | 예시 | 변경 규칙 |
| --- | --- | --- |
| Core | PROJECT_CONTEXT, EVAL_RUBRIC, REGRESSION_NOTES | 리뷰 필수 |
| Workflow | AI_WORKFLOW, workflows/* | 관련 사용자 1명 이상 확인 |
| Record | sessions/*, tasks/* | 작성자 책임으로 추가 가능 |
| Library | PROMPT_LIBRARY, examples/* | 성공 사례 링크 필요 |
| Deprecated | old docs | 읽지 말기 사유 표시 |

Core 문서는 AI 행동에 직접 영향을 준다. 따라서 변경이 작아 보여도 리뷰한다.

## 변경 승인 기준

하네스 문서 변경은 다음 기준으로 승인한다.

- 실제 세션, 이슈, 리뷰, 장애, 사용자 피드백 중 하나 이상의 근거가 있는가?
- AI의 다음 행동이 어떻게 달라져야 하는지 설명하는가?
- 특정 프로젝트에만 적용되는 규칙을 전역 규칙처럼 쓰지 않았는가?
- 기존 규칙과 충돌하지 않는가?
- 문서가 길어졌다면 검색성과 읽기성이 유지되는가?

이 기준은 특히 프롬프트 라이브러리에 중요하다. “좋아 보이는 프롬프트”가 아니라 “검증된 상황에서 효과가 있었던 프롬프트”만 남긴다.

## 하네스 변경 PR 예시

```text
제목:
인증 상태 구분 회귀 체크 추가

변경 이유:
최근 두 번의 AI 세션에서 만료 세션과 권한 없음 상태를 혼동했다.

근거:
- sessions/2026-05-28-role-guard.md
- sessions/2026-06-06-auth-empty-screen.md

변경 내용:
- GLOSSARY.md에 expired session, unauthorized, forbidden 정의 추가
- REGRESSION_NOTES.md에 인증 상태 구분 체크 추가
- EVAL_RUBRIC.md의 버그 수정 루브릭에 상태 구분 항목 추가

기대하는 AI 행동 변화:
인증 관련 작업에서 세 상태를 하나의 "로그인 문제"로 뭉뚱그리지 않고 별도 완료 조건으로 검증한다.

되돌릴 기준:
인증 정책 변경으로 세 상태 정의가 더 이상 맞지 않게 되면 폐기한다.
```

좋은 변경 PR은 하네스 문서를 바꾼 사실보다, AI 세션의 어떤 실패를 줄일지 설명한다.

## Stale 문서 처리

오래된 문서는 삭제보다 더 위험한 상태로 남을 수 있다. AI는 문서가 오래되었는지 스스로 확신하지 못한다.

문서 상태는 다음 중 하나로 둔다.

```text
상태: active
상태: draft
상태: deprecated
상태: archived
```

`deprecated` 문서 상단에는 반드시 이유와 대체 문서를 적는다.

```text
상태: deprecated
폐기일: 2026-06-06
이유: 2026년 3월 IdP 전환 이후 인증 흐름이 변경됨
대체 문서: decisions/2026-03-auth-routing.md
AI 주의: 이 문서를 현재 정책의 근거로 사용하지 말 것
```

삭제가 부담스럽다면 deprecated로 두되, 컨텍스트 패킷의 `읽지 말기`에 명시한다.

## 팀 회의 안건 템플릿

주간 하네스 리뷰는 다음 구조로 30분 안에 진행한다.

```text
# 주간 하네스 리뷰

## 1. 성공 세션

- 어떤 하네스 문서가 도움이 되었는가?
- 재사용할 프롬프트가 있는가?

## 2. 실패 세션

- 실패 원인은 Context Gap, Ambiguity Gap, Boundary Gap, Evaluation Gap 중 무엇인가?
- 하네스 문서 변경이 필요한가?

## 3. 변경 제안

- 추가할 규칙:
- 수정할 템플릿:
- 삭제할 낡은 문서:

## 4. 다음 주 실험

- 실험할 워크플로우:
- 성공 기준:
```

회의의 목적은 문서 품질 평가가 아니다. 실제 AI 세션의 마찰을 줄이는 것이다.

## 프로젝트별 override

팀 공용 템플릿과 프로젝트별 규칙은 분리한다.

```text
team-ai-harness/
  templates/
    TASK_CHARTER.md
    SESSION_LOG.md
    EVAL_RUBRIC.md

project-a/
  ai-harness/
    PROJECT_CONTEXT.md
    EVAL_RUBRIC.md
    REGRESSION_NOTES.md
```

팀 공용 템플릿은 시작점을 제공한다. 프로젝트별 하네스는 실제 위험과 용어를 담는다. 두 계층을 섞으면, 특정 프로젝트의 예외가 모든 프로젝트의 규칙처럼 퍼진다.

## 출처

- Awesome Harness Engineering human-in-the-loop: https://github.com/ai-boost/awesome-harness-engineering
- Awesome Harness Engineering memory and state: https://github.com/ai-boost/awesome-harness-engineering
- Ouroboros ledger and replayable workflow concepts: https://github.com/Q00/ouroboros
