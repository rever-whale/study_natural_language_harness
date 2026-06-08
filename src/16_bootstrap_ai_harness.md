# 16. 첫 번째 ai-harness 만들기

이 장은 자연어 하네스를 실제 프로젝트에 처음 설치하는 과정을 다룬다. 여기서 설치란 도구를 깔거나 코드를 작성하는 일이 아니다. 팀이 Claude나 Codex에게 반복해서 제공할 작업 환경을 문서로 만드는 일이다.

목표는 하루 안에 최소 하네스를 만드는 것이다. 완벽한 지식베이스가 아니라, 첫 AI 세션을 이전보다 덜 위험하게 시작할 수 있는 구조를 만든다.

## 0단계: 적용할 프로젝트 고르기

처음부터 가장 복잡한 핵심 시스템을 고르지 않는다. 다음 조건을 만족하는 프로젝트가 좋다.

- 팀이 자주 수정한다.
- AI를 이미 가끔 사용하고 있다.
- 반복되는 설명이나 실수가 있다.
- 프로젝트 맥락을 아는 사람이 옆에 있다.
- 실패해도 작은 작업부터 실험할 수 있다.

예를 들어 관리자 콘솔, 내부 운영 도구, 문서화가 부족한 프런트엔드 앱, 반복 버그가 많은 백엔드 서비스가 좋은 후보가 될 수 있다.

## 1단계: 폴더 만들기

프로젝트 루트나 문서 저장소에 다음 구조를 만든다.

```text
ai-harness/
  README.md
  PROJECT_CONTEXT.md
  GLOSSARY.md
  AI_WORKFLOW.md
  EVAL_RUBRIC.md
  REGRESSION_NOTES.md
  PROMPT_LIBRARY.md
  HARNESS_CHANGELOG.md
  tasks/
  sessions/
  decisions/
  workflows/
```

처음에는 대부분 비어 있어도 된다. 중요한 것은 팀이 같은 위치를 바라보는 것이다.

## 2단계: README 작성

`README.md`는 사람과 AI 모두를 위한 입구다.

```text
# ai-harness

이 폴더는 이 프로젝트에서 AI 세션을 안정적으로 운영하기 위한 팀 공용 하네스 지식베이스다.

## AI 세션 시작 순서

1. `PROJECT_CONTEXT.md`를 읽는다.
2. 작업 유형에 맞는 `AI_WORKFLOW.md` 절차를 확인한다.
3. 작업 헌장이 있으면 먼저 읽고 완료 조건을 요약한다.
4. `EVAL_RUBRIC.md`와 관련 `REGRESSION_NOTES.md`를 확인한다.
5. 모호한 점이 있으면 작업 전 질문한다.

## 문서 상태

- active: 현재 AI 세션에서 사용 가능
- draft: 사람이 검토 중이며 AI가 근거로 삼으면 안 됨
- deprecated: 오래되었거나 현재 정책과 충돌
```

이 README는 에이전트에게 “하네스가 존재한다”는 신호를 준다.

## 3단계: PROJECT_CONTEXT 1페이지 작성

처음 작성할 때는 깊이보다 위험 경계가 중요하다.

```text
# PROJECT_CONTEXT

상태: active
최종 검토일: 2026-06-06
소유자: admin-console-team

## 프로젝트 목적

고객사 관리자가 멤버, 권한, 결제 상태를 확인하고 관리하는 관리자 콘솔이다.

## 주요 사용자

- workspace admin: 멤버 초대, 권한 변경, 사용량 확인 가능
- member: 본인 정보와 일부 리포트만 확인 가능
- internal support: 고객 지원 목적으로 제한된 읽기 권한 보유

## 위험한 영역

- 권한 변경
- 결제 상태 표시
- 인증/세션 처리
- 감사 로그

## AI 작업 규칙

- 위험한 영역을 다룰 때는 작업 헌장 없이 바로 수정하지 않는다.
- 사용자에게 보이는 에러 문구는 임의로 바꾸지 않는다.
- 권한 관련 작업은 `REGRESSION_NOTES.md`를 먼저 확인한다.
```

## 4단계: 최소 평가 루브릭 작성

처음에는 공통 기준만 둔다.

```text
# EVAL_RUBRIC

## 공통 판정 기준

- 목표 적합성: 작업 헌장의 목표를 충족하는가?
- 범위 준수: 비목표와 금지 범위를 침범하지 않았는가?
- 프로젝트 일관성: 기존 용어와 구조를 유지하는가?
- 검증 가능성: 결과에 증거가 있는가?
- 운영 위험: 권한, 개인정보, 결제, 배포 위험이 식별되었는가?

## 판정

- Pass: 바로 수용 가능
- Needs Review: 사람이 특정 항목을 확인해야 함
- Revise: 재작업 필요
- Reject: 방향이 틀렸거나 위험함
```

## 5단계: 첫 작업 헌장 만들기

작은 작업을 하나 고른다. 예를 들어 “만료 세션 사용자가 관리자 페이지에 들어갈 때 에러 화면이 어색하다”는 이슈가 있다고 하자.

```text
# 작업 헌장: 만료 세션 에러 흐름 점검

배경:
만료된 세션으로 관리자 콘솔에 접근하면 빈 화면이 보인다는 제보가 있었다.

목표:
만료 세션, 권한 없음, 게스트 접근이 서로 다른 흐름으로 처리되는지 확인하고 개선 방향을 제안한다.

비목표:
- 인증 정책 자체를 변경하지 않는다.
- 에러 문구를 확정하지 않는다.
- 배포 가능한 코드를 바로 작성하지 않는다.

완료 조건:
- 세 가지 상태의 차이가 문서화된다.
- 현재 흐름의 위험이 정리된다.
- 수정이 필요하면 다음 작업 헌장 초안이 작성된다.

검증 방법:
- 관련 라우팅/인증 흐름 문서를 확인한다.
- `REGRESSION_NOTES.md`의 인증 항목을 확인한다.
```

## 6단계: 첫 세션 실행

Claude나 Codex에 다음처럼 시작한다.

```text
이 프로젝트는 `ai-harness/`를 팀 공용 하네스 지식베이스로 사용한다.
먼저 `ai-harness/README.md`, `PROJECT_CONTEXT.md`, `AI_WORKFLOW.md`, `EVAL_RUBRIC.md`를 읽어줘.
아직 작업하지 말고, 이 작업에서 지켜야 할 규칙을 요약해줘.
```

그다음 작업 헌장을 제공한다.

```text
이번 작업 헌장은 `ai-harness/tasks/2026-06-expired-session-flow.md`다.
작업 헌장을 읽고 완료 조건과 모호한 점을 정리해줘.
바로 수정하지 말고 질문이 있으면 먼저 물어봐.
```

## 7단계: 세션 로그 저장

첫 세션이 끝나면 결과가 좋든 나쁘든 로그를 남긴다.

```text
# sessions/2026-06-06-expired-session-flow.md

목표:
만료 세션, 권한 없음, 게스트 접근 흐름을 구분한다.

읽은 문서:
- PROJECT_CONTEXT.md
- EVAL_RUBRIC.md
- tasks/2026-06-expired-session-flow.md

중요한 발견:
- 팀 문서에 `권한 없음`과 `만료 세션`의 정의가 분리되어 있지 않다.

하네스 개선:
- GLOSSARY.md에 `expired session`, `unauthorized`, `forbidden` 정의를 추가해야 한다.
- REGRESSION_NOTES.md에 인증 상태 혼동 체크를 추가해야 한다.
```

첫 세션의 목적은 완벽한 결과물이 아니라, 하네스가 무엇을 모르는지 드러내는 것이다.

## 출처

- Awesome Harness Engineering: https://github.com/ai-boost/awesome-harness-engineering
- Ouroboros: https://github.com/Q00/ouroboros
- Prompt Engineering Guide: https://github.com/dair-ai/Prompt-Engineering-Guide
