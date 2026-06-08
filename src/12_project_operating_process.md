# 12. 프로젝트별 하네스 운영 절차

팀 공용 원칙이 있어도 프로젝트마다 하네스는 달라야 한다. 결제 시스템, 관리자 도구, 마케팅 페이지, 데이터 파이프라인은 AI에게 제공해야 할 맥락과 위험 기준이 다르다.

## 프로젝트 시작 시 만들 문서

새 프로젝트를 시작할 때는 최소 다섯 문서를 만든다.

```text
PROJECT_CONTEXT.md
GLOSSARY.md
AI_WORKFLOW.md
EVAL_RUBRIC.md
REGRESSION_NOTES.md
```

처음부터 완벽할 필요는 없다. 각 문서는 한 페이지 이하로 시작하고, 실제 세션에서 부족한 부분을 채운다.

## 작업 접수 절차

AI에게 맡길 작업은 바로 세션으로 보내지 않는다. 먼저 작업 헌장을 만든다.

1. 사람이 작업 요청을 적는다.
2. AI가 인터뷰 질문을 만든다.
3. 사람이 중요한 질문에 답한다.
4. AI가 작업 헌장 초안을 만든다.
5. 사람이 범위와 완료 조건을 승인한다.
6. 그다음 실행 세션으로 넘어간다.

작은 작업은 이 절차를 5분 안에 끝낼 수 있다. 큰 작업은 이 절차가 없으면 며칠을 잃을 수 있다.

## 세션 종료 절차

모든 의미 있는 AI 세션은 종료 기록을 남긴다.

```text
세션 종료 체크:

- 작업 목표를 달성했는가?
- 완료 조건별 증거가 있는가?
- 사람이 확인해야 할 항목은 무엇인가?
- 새로 발견한 프로젝트 규칙은 무엇인가?
- 하네스 문서에 반영할 변경은 무엇인가?
```

세션 로그가 쌓이면 팀은 어떤 프롬프트가 효과적인지, 어떤 작업은 AI에게 맡기면 위험한지 알게 된다.

## 하네스 품질 지표

자연어 하네스도 성과를 볼 수 있다.

- 작업 시작 전 확인 질문 수가 줄어드는가?
- 같은 종류의 실패가 줄어드는가?
- PR 리뷰에서 AI 결과물의 수정 요청이 줄어드는가?
- 세션마다 반복 설명하는 프로젝트 맥락이 줄어드는가?
- 신규 팀원이 AI 세션을 시작하는 시간이 줄어드는가?

정량 지표를 과하게 만들 필요는 없다. 중요한 것은 하네스가 실제 개발 흐름을 가볍게 만들고 있는지 보는 것이다.

## 출처

- Awesome Harness Engineering task runners and orchestration: https://github.com/ai-boost/awesome-harness-engineering
- Awesome Harness Engineering planning and task decomposition: https://github.com/ai-boost/awesome-harness-engineering
- Ouroboros specification-first workflow: https://github.com/Q00/ouroboros
