# B. 소스 맵

이 책은 세 가지 원천을 자연어 중심 실무 교재로 재구성했다.

## Prompt Engineering Guide

Prompt Engineering Guide는 프롬프트 요소, 기본 패턴, zero-shot, few-shot, chain-of-thought, prompt chaining, ReAct, RAG, 평가와 위험을 폭넓게 다룬다. 이 책에서는 그중 애플리케이션 개발자가 매일 쓰기 쉬운 패턴을 선별했다.

반영된 장:

- 2장: 프롬프트를 작업 인터페이스로 다루는 관점
- 3장: 기본 프롬프팅 패턴의 실무 재분류
- 4장: 실패 모드와 위험 관리
- 10장: 평가 프롬프트와 루브릭

## Awesome Harness Engineering

Awesome Harness Engineering은 하네스를 모델 밖의 실행 환경으로 본다. 컨텍스트 전달, 도구 설계, 권한, 메모리, 평가, 관찰, 사람 개입, 샌드박스 같은 주제를 정리한다. 이 책에서는 그 구조를 코드 없이 운영 문서와 팀 절차로 번역했다.

반영된 장:

- 5장: 하네스의 정의와 계층
- 8장: 컨텍스트 패킷과 지식베이스
- 9장: 실행 루프
- 10장: 평가와 회귀 체크
- 11장: 팀 공용 지식베이스
- 13장: 로컬 에이전트 권한과 세션 운영
- 14장: 하네스 자체의 관찰과 개선

## Ouroboros

Ouroboros는 명세 우선 워크플로우와 에이전트 OS 관점을 제공한다. 특히 막연한 요청을 인터뷰, 명세, 실행, 평가, 진화의 루프로 바꾸는 발상이 이 책의 핵심 구조에 영향을 주었다.

반영된 장:

- 6장: 작업 헌장
- 7장: Wonder와 Ontology 질문
- 9장: 실행 루프
- 12장: 프로젝트별 운영 절차
- 14장: 하네스 디벨롭 과정
- 16장: 첫 하네스 부트스트랩
- 17장: 인증 버그 사례의 진단 루프
- 18장: 신규 기능의 Seed Spec 흐름

## Part 4 실전 사례의 역할

초기 초안은 개념과 템플릿 중심이라 실제 프로젝트에 도입하는 과정이 부족했다. Part 4는 세 원천의 아이디어를 한 프로젝트의 운영 흐름으로 묶는다.

- Prompt Engineering Guide의 패턴은 실제 세션 프롬프트로 변환했다.
- Awesome Harness Engineering의 컨텍스트, 메모리, 평가, human-in-the-loop 요소는 팀 문서와 리뷰 절차로 변환했다.
- Ouroboros의 `interview -> spec -> run -> evaluate -> evolve` 흐름은 작업 헌장, 컨텍스트 패킷, 세션 로그, 하네스 변경 이력으로 변환했다.

## Part 5 자체 검증 루프의 역할

Part 5는 이 책의 작성 과정 자체를 하네스 대상으로 삼는다. mdBook 산출물도 자연어 하네스의 원칙을 따라야 하므로, `_state` 장부, 자체 리뷰 루브릭, 품질 게이트, 반복 보강 로그를 통해 책이 충분한 수준에 도달할 때까지 개선되도록 설계했다.

## 출처

- Prompt Engineering Guide repository: https://github.com/dair-ai/Prompt-Engineering-Guide
- Prompt Engineering Guide website: https://www.promptingguide.ai/
- Awesome Harness Engineering repository: https://github.com/ai-boost/awesome-harness-engineering
- Ouroboros repository: https://github.com/Q00/ouroboros
- Ouroboros Korean README: https://github.com/Q00/ouroboros/blob/main/README.ko.md
