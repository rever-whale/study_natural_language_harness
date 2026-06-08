# 18. 사례 연구: 신규 기능 명세 세션

버그 분석보다 더 어려운 작업은 신규 기능 명세다. 버그는 적어도 잘못된 상태가 보이지만, 신규 기능은 무엇을 만들지부터 흔들린다. 자연어 하네스는 신규 기능을 바로 구현하지 않고, 팀이 합의할 수 있는 명세로 좁힌다.

## 시작 요청

```text
관리자가 멤버 초대할 때 권한을 미리 지정할 수 있게 해줘.
```

이 요청은 단순해 보이지만 중요한 질문이 숨어 있다.

- 권한은 어떤 종류가 있는가?
- 초대받은 사용자가 가입하지 않으면 권한은 어디에 저장되는가?
- 기존 초대 링크와 충돌하지 않는가?
- 권한 변경 감사 로그는 언제 남는가?
- 잘못 초대했을 때 취소나 수정이 가능한가?
- 초대 수락 전에 권한 정책이 바뀌면 어떻게 되는가?

## Wonder 단계

먼저 문제 공간을 넓힌다.

```text
너는 제품 명세 인터뷰어다.
아직 구현 방식을 제안하지 말고, "관리자가 멤버 초대 시 권한을 미리 지정"한다는 요청의 목적과 대안을 탐색해줘.

Wonder 질문:
- 이 기능이 해결하려는 실제 문제는 무엇인가?
- 현재 초대 후 권한 변경 흐름에서 어떤 비용이 발생하는가?
- 초대 시 권한을 지정하지 않고도 문제를 해결할 방법은 있는가?
- 잘못된 권한 초대가 발생하면 어떤 위험이 있는가?
```

이 단계의 목표는 구현 아이디어를 확정하는 것이 아니라, 기능의 존재 이유를 분명히 하는 것이다.

## Ontology 단계

다음으로 용어를 좁힌다.

```text
Ontology 질문:
- "관리자"는 workspace admin인가 organization owner인가?
- "멤버"는 신규 가입자, 기존 사용자, 외부 게스트를 모두 포함하는가?
- "권한"은 role인가 permission set인가?
- "미리 지정"은 초대 생성 시점, 초대 수락 시점, 계정 생성 시점 중 언제 적용되는가?
- 초대가 만료되면 사전 지정 권한도 함께 폐기되는가?
```

이 질문에 답하지 않으면 AI는 익숙한 권한 모델을 마음대로 가져온다. 그것이 프로젝트의 실제 권한 체계와 다르면 위험하다.

## Seed Spec 만들기

Ouroboros의 명세 우선 흐름을 자연어로 옮기면 다음과 같은 Seed Spec이 된다.

```text
# Seed Spec: 초대 시 권한 사전 지정

## 문제

workspace admin은 신규 멤버를 초대한 뒤 별도 화면에서 권한을 조정해야 한다.
이 과정에서 멤버가 잘못된 기본 권한으로 잠시 접근하거나, 관리자가 권한 변경을 잊는 문제가 발생한다.

## 목표

workspace admin이 초대 생성 시 초대받을 멤버의 초기 role을 지정할 수 있게 한다.
초대 수락 후 멤버는 지정된 role로 workspace에 합류한다.

## 비목표

- role 체계를 재설계하지 않는다.
- organization owner 권한 정책을 변경하지 않는다.
- 초대 링크 보안 정책을 변경하지 않는다.
- 대량 초대 기능은 이번 범위에 포함하지 않는다.

## 핵심 용어

- inviter: 초대를 생성하는 workspace admin
- invitee: 초대를 수락할 사용자
- initial role: 초대 생성 시 지정되고, 수락 시 적용되는 role
- pending invite: 아직 수락되지 않은 초대

## 완료 조건

- 초대 생성 시 role을 선택할 수 있다.
- 선택 가능한 role은 inviter의 권한보다 높을 수 없다.
- 초대 수락 시 initial role이 적용된다.
- 초대 취소 시 pending role 정보도 폐기된다.
- 감사 로그에는 초대 생성자, invitee, initial role이 남는다.

## 남은 질문

- 초대 수락 전에 role 정책이 변경되면 pending invite를 무효화할 것인가?
- 기존 pending invite의 role을 수정할 수 있어야 하는가?
```

Seed Spec은 구현 지시서가 아니다. 팀이 논의할 수 있는 최소 명세다.

## 컨텍스트 패킷

```text
반드시 읽기:
- PROJECT_CONTEXT.md
- GLOSSARY.md
- decisions/2026-02-role-model.md
- EVAL_RUBRIC.md

필요하면 읽기:
- sessions/2026-04-member-invite-flow.md
- workflows/release-checklist.md

읽지 말기:
- docs/legacy-permission-matrix.md: 현재 role 모델 이전 문서

주의:
- role 변경은 감사 로그와 연결된다.
- 결제 plan에 따라 초대 가능한 role이 달라질 수 있다.
```

## 설계 옵션 만들기

명세가 어느 정도 선명해지면 AI에게 옵션을 만들게 한다.

```text
Seed Spec을 기준으로 설계 옵션을 3개 제시해줘.
각 옵션마다 다음을 포함해줘:
- 사용자 흐름
- 데이터 계약 변화
- 권한/감사 로그 영향
- 장점
- 위험
- 먼저 검증해야 할 질문
```

좋은 응답은 옵션을 단순 UI 차이가 아니라 운영 위험 차이로 나눈다.

```text
옵션 A: 초대 생성 시 role을 필수 선택
- 장점: 누락이 없다.
- 위험: 단순 초대 흐름이 복잡해진다.

옵션 B: 기본 role을 유지하되 고급 설정으로 role 선택
- 장점: 기존 흐름 영향이 작다.
- 위험: 관리자가 여전히 설정을 놓칠 수 있다.

옵션 C: 초대 템플릿을 만들어 role과 메시지를 함께 저장
- 장점: 반복 초대에 유리하다.
- 위험: 이번 범위를 크게 넘는다.
```

## 평가와 결정

팀은 평가 루브릭으로 옵션을 고른다.

```text
판정:
옵션 B를 1차 범위로 선택한다.

이유:
- 기존 초대 흐름의 마찰을 최소화한다.
- 권한 지정 기능을 제공하되 필수 입력으로 만들지 않는다.
- 대량 초대나 템플릿 기능으로 범위가 커지는 것을 막는다.

후속 확인:
- 기본 role이 무엇인지 정책 확인 필요
- 초대 수락 전 role 수정 가능 여부는 별도 작업으로 분리
```

좋은 명세 세션은 모든 결정을 끝내지 않는다. 어떤 결정을 이번에 하고, 어떤 결정을 다음 작업으로 넘길지 분리한다.

## 하네스 업데이트

신규 기능 명세 세션 후에는 다음 문서를 업데이트한다.

- `GLOSSARY.md`: inviter, invitee, initial role, pending invite
- `decisions/`: 선택한 설계 옵션과 이유
- `tasks/`: 구현 작업 헌장
- `EVAL_RUBRIC.md`: 권한 기능 명세 루브릭 보강
- `REGRESSION_NOTES.md`: 권한 상승 위험 체크 추가

자연어 하네스의 핵심은 명세 세션의 산출물이 다음 구현 세션의 입력이 되도록 연결하는 것이다.

## 출처

- Ouroboros README: https://github.com/Q00/ouroboros
- Prompt Engineering Guide prompt chaining: https://www.promptingguide.ai/techniques/prompt_chaining
- Awesome Harness Engineering planning and task decomposition: https://github.com/ai-boost/awesome-harness-engineering
