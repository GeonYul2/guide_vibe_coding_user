# Agent Engineering Guide — User Distribution

이 레포는 **자동화 에이전트를 설계·검증하기 위한 사용자용 helper 배포본**입니다. Maintainer 전용 GitHub radar, technique discovery, publish workflow, 전체 generated wiki는 포함하지 않고, 대신 토큰 절약용 curated `wiki/`를 포함합니다.

## 반드시 이렇게 시작하세요

사용자가 새 자동화 에이전트를 만들고 싶다고 말하면, 에이전트는 **다른 설명을 길게 시작하지 말고 먼저 아래 한 문장만 물어야 합니다.**

> 토큰 절감모드로 시작할까요?

그 답을 받은 뒤에만 인터뷰를 시작합니다. 인터뷰는 한 번에 긴 양식을 던지지 않고, `workflows/intake.md`에 따라 **한 번에 한 질문씩** 진행하며 각 답을 `tasks/<task-slug>/intake-form.md`에 누적합니다.

## 빠른 사용법

```bash
python3 scripts/new_agent_task.py "internal-doc-report-agent"
python3 scripts/validate_agent_task.py tasks/internal-doc-report-agent
```

에이전트는 먼저 `wiki/index.md`로 필요한 문맥을 좁힌 뒤 `AGENTS.md`, `agent-playbook.yaml`, `workflows/build-agent.md`, `techniques/taxonomy.yaml`, `techniques/registry.yaml`, `templates/`를 원본 기준으로 검증합니다.

## 사용자가 “어떤 테크닉들이 있어?”라고 물으면

개발자가 아니어도 이해할 수 있도록, 먼저 아래처럼 **목록표로 설명**합니다. 세부 ID나 YAML 이름은 필요할 때만 덧붙입니다.

| 쉬운 이름 | 무엇을 도와주나 | 언제 특히 중요한가 |
| --- | --- | --- |
| 시작 질문 정리 | 만들 에이전트의 목적, 사용자, 입력, 출력, 성공 기준을 빠뜨리지 않게 묻습니다. | 모든 새 에이전트 작업 |
| 깊은 인터뷰 | 애매한 요구사항을 구체적인 업무 흐름과 승인 경계로 바꿉니다. | “대충 자동화해줘”처럼 범위가 흐릴 때 |
| 출력 형식 검증 | 결과를 JSON, YAML, Markdown 등 정해진 형식으로 안정적으로 내보내게 합니다. | 결과를 다른 도구가 읽거나 저장할 때 |
| 테스트와 회귀 방지 | 성공/실패 사례를 테스트로 남겨 나중에 품질이 떨어지지 않게 합니다. | 반복 실행하거나 운영에 넣을 때 |
| 실패 사례 기억 | 애매한 입력, 도구 오류, 안전 이슈를 기록해 다음 개선에 반영합니다. | 실사용 중 문제가 생길 수 있을 때 |
| 안전장치와 중단선 | 위험한 요청, 외부 발송, 삭제, 결제, 권한 변경을 멈추거나 승인받게 합니다. | 사람·돈·데이터에 영향을 줄 때 |
| 도구 사용 계약 | API, 파일, DB, 브라우저 같은 도구를 어떤 권한과 규칙으로 쓸지 정합니다. | 에이전트가 실제 시스템을 조작할 때 |
| 권한 있는 실행 | 자동 실행 가능한 일과 사람 승인이 필요한 일을 분리합니다. | 업무 데이터 변경, 외부 메시지 발송, 배포가 있을 때 |
| 보안·개인정보 보호 | 회사/고객 데이터, 인증정보, 로그를 최소 권한·마스킹·보관 규칙으로 다룹니다. | 내부 문서, 고객정보, 계정 권한을 다룰 때 |
| 비용·토큰 예산 | 한 번 실행에 쓸 수 있는 토큰·비용·캐시 기준을 정합니다. | 반복 업무, 대량 문서, 유료 모델 호출이 있을 때 |
| 모델 선택과 대체 | 쉬운 일은 저렴한 모델, 어려운 일은 강한 모델로 보내고 실패 시 대체 경로를 둡니다. | 비용과 품질을 함께 관리해야 할 때 |
| 관찰·로그·텔레메트리 | 무엇을 판단했고 어떤 도구를 썼고 비용이 얼마였는지 추적합니다. | 운영, 감사, 품질 개선이 필요할 때 |
| 배포·카나리·롤백 | 작게 실험하고, 문제가 생기면 멈추고 되돌리는 절차를 둡니다. | 실제 사용자나 운영 환경에 배포할 때 |
| 짧은 운영 계약 | 에이전트가 어떻게 말하고, 언제 묻고, 언제 멈출지 짧고 강하게 정합니다. | 팀이 반복해서 같은 에이전트를 쓸 때 |
| 범위 통제 | 첫 버전에서 하지 않을 일을 명확히 하고 작은 변경만 하게 합니다. | 기능 욕심으로 프로젝트가 커질 때 |
| 문서·지식 관리 | 위키, 문서, 검색 메모리를 기준으로 근거 있는 답을 만들게 합니다. | 내부 문서나 지식 기반을 쓰는 에이전트 |

정확한 전체 technique ID와 선택/거절 사유는 최종적으로 `techniques/taxonomy.yaml`, `techniques/registry.yaml`, 그리고 각 태스크의 `technique-selection.yaml`에서 확인합니다.

## 에이전트를 다 만든 뒤 설명해야 하는 것

완성된 에이전트는 “작동한다”에서 끝나지 않고, **어떤 테크닉이 왜 적용됐는지 설명 가능해야 합니다.** 최종 보고에는 최소한 아래를 포함합니다.

- 적용한 테크닉: 선택한 이유와 사용된 산출물
- 적용하지 않은 테크닉: 제외한 이유
- 사용자가 이해할 수 있는 쉬운 설명: 개발 용어 대신 업무 효과 중심
- 검증 증거: validator, 테스트/eval, 체크리스트, 로그 또는 메트릭
- 남은 위험: 아직 사람 승인이 필요한 부분, 운영 전 확인할 부분

## 사용자 경계

- 새 작업은 토큰 절감모드 시작 질문 뒤 `tasks/<task-slug>/intake-form.md`부터 작성합니다.
- intake는 질문-답변 흐름으로 진행하고, 전체 12개 항목을 첫 메시지에 던지지 않습니다.
- 구현 전 필수 산출물을 모두 채우고 validator를 통과해야 합니다.
- 이 배포본은 GitHub technique radar를 실행하지 않습니다.
- technique 업데이트/레이더/배포 publish는 source/maintainer 레포에서만 관리합니다.

## User Wiki

- 시작점: `wiki/index.md`
- technique 선택 지도: `wiki/technique-map.md`
- intake/build 흐름 요약: `wiki/workflows/`

`wiki/`는 토큰 절약용 지도이고 최종 근거가 아닙니다. 최종 선택/거절 이유는 반드시 `techniques/taxonomy.yaml`, `techniques/registry.yaml`, task-local `technique-selection.yaml`에서 검증합니다.

## 핵심 파일

- `AGENTS.md` — 사용자 배포본용 에이전트 실행 계약
- `agent-playbook.yaml` — 필수 산출물과 게이트의 기계 판독 정의
- `workflows/` — intake, deep interview, build-agent 흐름
- `wiki/` — 토큰 절약용 user wiki와 technique 선택 지도
- `templates/` — task 산출물 템플릿
- `techniques/` — 선택/거절해야 하는 agent-engineering technique registry
- `scripts/new_agent_task.py` — task scaffold 생성
- `scripts/validate_agent_task.py` — readiness/completion artifact 검증

## 보안 경계

이 배포본은 표준 라이브러리 Python 스크립트와 Markdown/YAML만 포함합니다. GitHub Actions, maintainer radar, 외부 저장소 clone/install/execute 로직은 포함하지 않습니다.
