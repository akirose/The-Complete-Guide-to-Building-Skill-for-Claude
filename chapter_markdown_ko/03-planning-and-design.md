# 계획과 설계

## 사용 사례부터 시작하기

코드를 작성하기 전에, 스킬이 지원해야 할 구체적인 사용 사례를 2~3개 먼저 정의하세요.

### 좋은 사용 사례 정의 예시

사용 사례: 프로젝트 스프린트 계획
트리거: 사용자가 "이번 스프린트 계획을 도와줘" 또는 "스프린트 작업 만들어줘"라고 말함
단계:
1. Linear에서 현재 프로젝트 상태 조회(MCP 경유)
2. 팀 속도와 수용 용량 분석
3. 작업 우선순위 제안
4. 적절한 라벨과 추정치를 넣어 Linear 작업 생성
결과: 작업이 생성된 완전한 스프린트 계획

### 자문할 질문
- 사용자가 달성하려는 목표는 무엇인가?
- 이를 위해 어떤 다단계 워크플로가 필요한가?
- 어떤 도구가 필요한가?(내장 도구 또는 MCP)
- 어떤 도메인 지식/모범 사례를 스킬에 내장해야 하는가?

## 일반적인 스킬 사용 사례 카테고리

Anthropic에서 관찰한 공통 카테고리는 다음 3가지입니다.

### 카테고리 1: 문서 및 자산 생성

용도: 문서, 프레젠테이션, 앱, 디자인, 코드 등 일관되고 품질 높은 산출물 생성

실제 예시: `frontend-design` 스킬(또한 docx, pptx, xlsx, ppt 스킬 참고)

"높은 디자인 품질의 개성 있는 프로덕션급 프론트엔드 인터페이스를 생성한다. 웹 컴포넌트, 페이지, 아티팩트, 포스터, 애플리케이션을 만들 때 사용한다."

#### 핵심 기법
- 스타일 가이드와 브랜드 기준 내장
- 일관된 결과를 위한 템플릿 구조
- 최종화 전 품질 체크리스트
- 외부 도구 불필요(클로드 내장 기능 사용)

### 카테고리 2: 워크플로 자동화

용도: 일관된 방법론이 중요한 다단계 프로세스(여러 MCP 서버 조정 포함)

실제 예시: `skill-creator` 스킬

"새 스킬 생성을 위한 대화형 가이드. 사용 사례 정의, 프런트매터 생성, 지침 작성, 검증까지 안내한다."

#### 핵심 기법
- 검증 게이트가 있는 단계별 워크플로
- 공통 구조 템플릿
- 내장된 검토 및 개선 제안
- 반복 개선 루프

### 카테고리 3: MCP 강화

용도: MCP 서버가 제공하는 도구 접근을 실제 워크플로 지침으로 강화

실제 예시: `sentry-code-review` 스킬(Sentry)

"Sentry MCP 서버의 오류 모니터링 데이터를 사용해 GitHub PR에서 감지된 버그를 자동 분석/수정한다."

#### 핵심 기법
- 여러 MCP 호출을 순차 조정
- 도메인 전문성 내장
- 사용자가 매번 지정해야 할 문맥을 기본 제공
- 흔한 MCP 오류 처리

## 성공 기준 정의하기

스킬이 잘 동작한다고 어떻게 판단할 것인가?

아래 수치는 엄밀한 절대 기준이라기보다 지향점에 가깝습니다. 엄격함을 추구하되, 어느 정도는 감각 기반 평가가 섞일 수 있습니다. 보다 견고한 측정 가이드와 도구는 계속 발전 중입니다.

### 정량 지표
- 관련 질의의 90%에서 스킬이 트리거됨
  - 측정 방법: 트리거되어야 하는 테스트 질의 10~20개를 실행하고, 자동 로드 비율과 명시 호출 필요 비율을 기록
- 워크플로 완료에 필요한 도구 호출 수(X회)
  - 측정 방법: 스킬 사용/미사용 동일 작업을 비교해 호출 수와 총 토큰 사용량 측정
- 워크플로당 실패 API 호출 0회
  - 측정 방법: 테스트 중 MCP 서버 로그에서 재시도율과 오류 코드를 추적

### 정성 지표
- 사용자가 다음 단계를 따로 프롬프트하지 않아도 됨
  - 평가 방법: 테스트 중 리다이렉트/재설명 빈도 기록, 베타 사용자 피드백 수집
- 사용자 수정 없이 워크플로 완료
  - 평가 방법: 같은 요청을 3~5회 실행하고 구조적 일관성/품질 비교
- 세션 간 결과 일관성
  - 평가 방법: 신규 사용자도 최소한의 안내로 첫 시도에 과업을 완료할 수 있는지 확인

## 기술 요구사항

### 파일 구조

```
your-skill-name/
├── SKILL.md              # 필수 - 메인 스킬 파일
├── scripts/              # 선택 - 실행 코드
│   ├── process_data.py   # 예시
│   └── validate.sh       # 예시
├── references/           # 선택 - 문서
│   ├── api-guide.md      # 예시
│   └── examples/         # 예시
└── assets/               # 선택 - 템플릿 등
    └── report-template.md # 예시
```

### 중요 규칙

`SKILL.md` 파일명:
- 정확히 `SKILL.md`여야 함(대소문자 구분)
- 변형 불가(`SKILL.MD`, `skill.md` 등 불가)

스킬 폴더명:
- 케밥 케이스 사용: `notion-project-setup` ✅
- 공백 금지: `Notion Project Setup` ❌
- 밑줄 금지: `notion_project_setup` ❌
- 대문자 금지: `NotionProjectSetup` ❌

`README.md` 금지:
- 스킬 폴더 내부에는 `README.md`를 두지 않음
- 문서는 `SKILL.md` 또는 `references/`에 작성
- 단, GitHub 배포 시 저장소 루트에는 사람 독자를 위한 README를 두는 것이 좋음(배포 섹션 참고)

### YAML 프런트매터: 가장 중요한 부분

클로드가 스킬을 로드할지 판단하는 근거가 YAML 프런트매터입니다. 이 부분을 정확히 작성해야 합니다.

#### 최소 필수 형식

```yaml
---
name: your-skill-name
description: What it does. Use when user asks to [specific phrases].
---
```

시작은 이것만으로 충분합니다.

#### 필드 요구사항

`name` (필수):
- 케밥 케이스만 허용
- 공백/대문자 금지
- 폴더명과 일치 권장

`description` (필수):
- 반드시 둘 다 포함
  - 스킬이 무엇을 하는지
  - 언제 써야 하는지(트리거 조건)
- 1024자 이하
- XML 태그 금지(`<`, `>`)
- 사용자가 실제로 말할 법한 구체 작업 문구 포함
- 관련 있다면 파일 형식 언급

`license` (선택):
- 오픈소스로 공개할 때 사용
- 보편 예시: `MIT`, `Apache-2.0`

`compatibility` (선택):
- 1~500자
- 환경 요구사항 표기(대상 제품, 필요한 시스템 패키지, 네트워크 접근 요구 등)

`metadata` (선택):
- 임의의 키-값 확장 가능
- 권장 키: `author`, `version`, `mcp-server`
- 예시:

```yaml
metadata:
  author: ProjectHub
  version: 1.0.0
  mcp-server: projecthub
```

#### 보안 제한

프런트매터에서 금지:
- XML 꺾쇠 괄호(`< >`)
- 이름에 `claude` 또는 `anthropic` 포함된 스킬(예약어)

이유: 프런트매터는 클로드의 시스템 프롬프트에 포함되므로 악성 내용이 지침 주입을 일으킬 수 있습니다.

## 효과적인 스킬 작성법

### `description` 필드

Anthropic 엔지니어링 블로그의 설명:
"이 메타데이터는 전체 내용을 문맥에 로드하지 않고도, 각 스킬을 언제 사용해야 하는지 판단할 최소 정보를 제공한다."
즉, 점진적 공개의 1단계입니다.

#### 구조

`[무엇을 하는가] + [언제 사용하는가] + [핵심 기능]`

#### 좋은 설명 예시

```text
# Good - 구체적이고 실행 가능
description: Analyzes Figma design files and generates developer handoff documentation. Use when user uploads .fig files, asks for "design specs", "component documentation", or "design-to-code handoff".

# Good - 트리거 문구 포함
description: Manages Linear project workflows including sprint planning, task creation, and status tracking. Use when user mentions "sprint", "Linear tasks", "project planning", or asks to "create tickets".

# Good - 가치 제안이 명확
description: End-to-end customer onboarding workflow for PayFlow. Handles account creation, payment setup, and subscription management. Use when user says "onboard new customer", "set up subscription", or "create PayFlow account".
```

#### 나쁜 설명 예시

```text
# 너무 모호함
description: Helps with projects.

# 트리거 누락
description: Creates sophisticated multi-page documentation systems.

# 기술 설명만 있고 사용자 트리거 없음
description: Implements the Project entity model with hierarchical relationships.
```

### 본 지침 작성하기

프런트매터 뒤에 실제 지침을 마크다운으로 작성합니다.

#### 권장 구조

다음 템플릿에서 대괄호 부분을 자신의 내용으로 바꿔 사용하세요.

```markdown
---
name: your-skill
description: [--.]
---

# Your Skill Name

## Instructions

### Step 1: [First Major Step]
무슨 일이 일어나는지 명확히 설명.

예시:
```bash
python scripts/fetch_data.py --project-id PROJECT_ID
```

Expected output: [성공 시 기대 결과 설명]

(필요한 만큼 단계 추가)

## Examples

Example 1: [common scenario]
User says: "Set up a new marketing campaign"
Actions:
1. Fetch existing campaigns via MCP
2. Create new campaign with provided parameters
Result: Campaign created with confirmation link

(필요한 만큼 예시 추가)

## Troubleshooting

Error: [Common error message]
Cause: [Why it happens]
Solution: [How to fix]

(필요한 만큼 오류 사례 추가)
```

### 지침 작성 모범 사례

#### 구체적이고 실행 가능하게 쓰기

✅ Good:

`python scripts/validate.py --input {filename}`를 실행해 데이터 형식을 확인한다.
검증 실패 시 흔한 문제:
- 필수 필드 누락(CSV에 필드 추가)
- 날짜 형식 오류(`YYYY-MM-DD` 사용)

❌ Bad:

진행 전에 데이터를 검증한다.

#### 오류 처리를 포함하기

```markdown
## Common Issues

### MCP Connection Failed
"Connection refused"가 보이면:
1. MCP 서버 실행 여부 확인: Settings > Extensions
2. API 키 유효성 확인
3. 재연결 시도: Settings > Extensions > [Your Service] > Reconnect
```

#### 번들 리소스를 명확히 참조하기

쿼리 작성 전에 `references/api-patterns.md`를 참고하도록 안내하세요.
포함할 내용 예:
- 레이트 리밋 가이드
- 페이지네이션 패턴
- 오류 코드와 처리 방식

#### 점진적 공개 활용하기

`SKILL.md`는 핵심 지침에 집중하고, 상세 문서는 `references/`로 분리한 뒤 링크하세요. (3단계 구조는 핵심 설계 원칙 절 참고)
