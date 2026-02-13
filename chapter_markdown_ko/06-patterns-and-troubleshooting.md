# 패턴과 문제 해결

아래 패턴은 초기 도입 사용자와 내부 팀이 만든 스킬에서 도출된 것입니다. 반드시 따라야 하는 고정 템플릿이 아니라, 실제로 효과가 좋았던 공통 접근법입니다.

## 접근 방식 선택: 문제 중심 vs 도구 중심

홈디포에 간 상황을 떠올리면 쉽습니다. "주방 수납장을 고쳐야 해"라는 문제를 들고 가면 직원이 필요한 도구를 안내해 줍니다. 반대로 새 드릴을 먼저 고른 뒤, 내 작업에 어떻게 쓰는지 물을 수도 있습니다.

스킬도 동일합니다.
- 문제 중심: "프로젝트 작업공간을 설정해야 해" → 스킬이 올바른 MCP 호출을 올바른 순서로 오케스트레이션. 사용자는 결과를 설명하고, 도구 처리는 스킬이 담당.
- 도구 중심: "Notion MCP는 연결해 뒀어" → 스킬이 최적 워크플로와 모범 사례를 가르침. 사용자는 접근 권한이 있고, 스킬은 전문성을 제공.

대부분의 스킬은 한쪽에 더 기웁니다. 내 사용 사례에 맞는 프레이밍을 먼저 정하면, 아래 패턴 선택이 쉬워집니다.

## 패턴 1: 순차 워크플로 오케스트레이션

사용 시점: 사용자가 정해진 순서의 다단계 프로세스를 필요로 할 때

### 예시 구조

```markdown
## Workflow: Onboard New Customer

### Step 1: Create Account
Call MCP tool: `create_customer`
Parameters: name, email, company

### Step 2: Setup Payment
Call MCP tool: `setup_payment_method`
Wait for: payment method verification

### Step 3: Create Subscription
Call MCP tool: `create_subscription`
Parameters: plan_id, customer_id (from Step 1)

### Step 4: Send Welcome Email
Call MCP tool: `send_email`
Template: welcome_email_template
```

### 핵심 기법
- 단계 순서를 명시적으로 고정
- 단계 간 의존성 표현
- 단계별 검증 수행
- 실패 시 롤백 지침 제공

## 패턴 2: 다중 MCP 조정

사용 시점: 워크플로가 여러 서비스를 가로지를 때

### 예시: 디자인에서 개발로의 핸드오프

```markdown
### Phase 1: Design Export (Figma MCP)
1. Figma에서 디자인 자산 내보내기
2. 디자인 명세 생성
3. 자산 매니페스트 생성

### Phase 2: Asset Storage (Drive MCP)
1. Drive에 프로젝트 폴더 생성
2. 모든 자산 업로드
3. 공유 링크 생성

### Phase 3: Task Creation (Linear MCP)
1. 개발 작업 생성
2. 작업에 자산 링크 첨부
3. 엔지니어링 팀에 할당

### Phase 4: Notification (Slack MCP)
1. #engineering 채널에 핸드오프 요약 게시
2. 자산 링크와 작업 참조 포함
```

### 핵심 기법
- 단계(Phase) 경계를 명확히 분리
- MCP 간 데이터 전달 설계
- 다음 단계 이동 전 검증
- 오류 처리를 중앙화

## 패턴 3: 반복 정제

사용 시점: 반복할수록 출력 품질이 좋아지는 작업

### 예시: 보고서 생성

```markdown
## Iterative Report Creation

### Initial Draft
1. MCP로 데이터 조회
2. 보고서 초안 생성
3. 임시 파일에 저장

### Quality Check
1. 검증 스크립트 실행: `scripts/check_report.py`
2. 이슈 식별:
   - 누락된 섹션
   - 불일치한 서식
   - 데이터 검증 오류

### Refinement Loop
1. 식별된 각 이슈 수정
2. 영향받은 섹션 재생성
3. 재검증
4. 품질 임계치 충족까지 반복

### Finalization
1. 최종 서식 적용
2. 요약 생성
3. 최종본 저장
```

### 핵심 기법
- 품질 기준 명시
- 반복 개선 루프 운영
- 검증 스크립트 사용
- 반복 종료 시점 기준 정의

## 패턴 4: 문맥 인지 도구 선택

사용 시점: 같은 목표라도 문맥에 따라 써야 할 도구가 달라질 때

### 예시: 파일 저장

```markdown
## Smart File Storage

### Decision Tree
1. 파일 형식과 크기 확인
2. 최적 저장 위치 결정:
   - 대용량 파일(>10MB): 클라우드 저장 MCP
   - 협업 문서: Notion/Docs MCP
   - 코드 파일: GitHub MCP
   - 임시 파일: 로컬 저장소

### Execute Storage
결정 결과에 따라:
- 적절한 MCP 도구 호출
- 서비스별 메타데이터 적용
- 접근 링크 생성

### Provide Context to User
왜 해당 저장소를 선택했는지 사용자에게 설명
```

### 핵심 기법
- 의사결정 기준을 명확히 정의
- 대체 경로(fallback) 준비
- 선택 근거를 투명하게 전달

## 패턴 5: 도메인 특화 지능

사용 시점: 단순 도구 접근을 넘어, 전문 지식을 스킬이 제공해야 할 때

### 예시: 금융 컴플라이언스

```markdown
## Payment Processing with Compliance

### Before Processing (Compliance Check)
1. MCP로 거래 상세 조회
2. 컴플라이언스 규칙 적용:
   - 제재 목록 확인
   - 관할권 허용 여부 검증
   - 위험도 평가
3. 컴플라이언스 판단 기록

### Processing
IF compliance passed:
- 결제 처리 MCP 도구 호출
- 적절한 사기 탐지 검사 적용
- 거래 처리
ELSE:
- 검토 대상으로 표시
- 컴플라이언스 케이스 생성

### Audit Trail
- 모든 컴플라이언스 점검 로그 기록
- 처리 의사결정 기록
- 감사 보고서 생성
```

### 핵심 기법
- 로직에 도메인 전문성 내장
- 실행 전 컴플라이언스 선검증
- 문서화 완결성 확보
- 거버넌스 기준 명확화

## 문제 해결

### 스킬 업로드가 안 될 때

오류: `Could not find SKILL.md in uploaded folder`
원인: 파일명이 정확히 `SKILL.md`가 아님
해결:
- 파일명을 `SKILL.md`로 변경(대소문자 구분)
- `ls -la` 결과에 `SKILL.md`가 보이는지 확인

오류: `Invalid frontmatter`
원인: YAML 형식 오류

흔한 실수:

```yaml
# 잘못됨 - 구분자 누락
name: my-skill
description: Does things

# 잘못됨 - 따옴표 미종결
name: my-skill
description: "Does things

# 올바름
---
name: my-skill
description: Does things
---
```

오류: `Invalid skill name`
원인: 이름에 공백 또는 대문자 포함

```yaml
# 잘못됨
name: My Cool Skill

# 올바름
name: my-cool-skill
```

### 스킬이 트리거되지 않을 때

증상: 자동 로드가 전혀 되지 않음

해결:
`description` 필드를 수정하세요. 좋은/나쁜 예시는 `The Description Field` 절을 참고하세요.

빠른 점검 목록:
- 너무 일반적인가?(`Helps with projects`는 동작하기 어려움)
- 사용자가 실제로 말할 트리거 문구가 포함돼 있는가?
- 필요 시 관련 파일 형식이 언급돼 있는가?

디버깅 접근:

클로드에게 `When would you use the [skill name] skill?`라고 물으면, 클로드가 설명 필드를 인용해 답합니다. 빠진 정보를 기준으로 설명을 조정하세요.

### 스킬이 너무 자주 트리거될 때

증상: 무관한 질의에서도 로드됨

해결책 1: 부정 트리거 추가

```text
description: Advanced data analysis for CSV files. Use for statistical modeling, regression, clustering. Do NOT use for simple data exploration (use data-viz skill instead).
```

해결책 2: 범위를 더 구체화

```text
# 너무 넓음
description: Processes documents

# 더 구체적
description: Processes PDF legal documents for contract review
```

해결책 3: 스코프 명확화

```text
description: PayFlow payment processing for e-commerce. Use specifically for online payment workflows, not for general financial queries.
```

### MCP 연결 이슈

증상: 스킬은 로드되지만 MCP 호출 실패

점검 목록:
1. MCP 서버 연결 확인
   - Claude.ai: `Settings > Extensions > [Your Service]`
   - 상태가 `Connected`인지 확인
2. 인증 확인
   - API 키 유효/만료 여부
   - 필요한 권한/스코프 부여 여부
   - OAuth 토큰 갱신 여부
3. MCP 단독 테스트
   - 스킬 없이 MCP를 직접 호출하게 요청
   - 예: `Use [Service] MCP to fetch my projects`
   - 이 단계가 실패하면 문제는 스킬이 아니라 MCP
4. 도구 이름 검증
   - 스킬이 참조하는 MCP 도구명이 정확한지
   - MCP 서버 문서와 일치하는지
   - 대소문자 구분 여부 확인

### 지침이 지켜지지 않을 때

증상: 스킬은 로드되지만 지침을 따르지 않음

흔한 원인:
1. 지침이 너무 장황함
   - 지침을 간결하게
   - 글머리표/번호 목록 활용
   - 상세 참고는 별도 파일로 분리
2. 핵심 지침이 묻힘
   - 중요한 지침을 상단에 배치
   - `## Important`, `## Critical` 같은 제목 사용
   - 필요하면 핵심 포인트 반복
3. 표현이 모호함

```text
# 나쁨
Make sure to validate things properly

# 좋음
CRITICAL: Before calling create_project, verify:
- Project name is non-empty
- At least one team member assigned
- Start date is not in the past
```

고급 기법: 매우 중요한 검증은 자연어 지시만 믿기보다 스크립트를 번들로 포함해 프로그램적으로 검사하는 방식을 고려하세요. 코드는 결정적이지만 자연어 해석은 그렇지 않습니다. 이 패턴은 Office 스킬 예시에서 확인할 수 있습니다.

4. 모델의 "게으른 실행" 완화

```markdown
## Performance Notes
- Take your time to do this thoroughly
- Quality is more important than speed
- Do not skip validation steps
```

참고: 이런 문구는 `SKILL.md`보다 사용자 프롬프트에 넣을 때 효과가 더 큰 경우가 많습니다.

### 대형 문맥 이슈

증상: 스킬 응답이 느려지거나 품질 저하

원인:
- 스킬 콘텐츠 과다
- 동시에 활성화된 스킬 과다
- 점진적 공개가 아닌 전체 로드

해결:
1. `SKILL.md` 크기 최적화
   - 상세 문서는 `references/`로 이동
   - 본문 인라인 대신 링크 참조
   - `SKILL.md`를 5,000단어 이하로 유지
2. 활성 스킬 수 줄이기
   - 동시에 20~50개 이상 켜져 있는지 점검
   - 선택적 활성화를 권장
   - 연관 기능은 스킬 팩 형태 고려
