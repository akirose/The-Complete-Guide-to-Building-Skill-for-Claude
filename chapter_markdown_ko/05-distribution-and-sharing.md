# 배포와 공유

스킬은 MCP 통합을 더 완성된 형태로 만듭니다. 사용자가 커넥터를 비교할 때, 스킬이 있는 쪽이 가치 실현까지의 경로가 더 짧아 MCP만 제공하는 대안 대비 우위를 가질 수 있습니다.

## 현재 배포 모델(2026년 1월)

개별 사용자가 스킬을 받는 방법:
1. 스킬 폴더 다운로드
2. 필요 시 폴더를 zip으로 압축
3. Claude.ai에서 `Settings > Capabilities > Skills`로 업로드
4. 또는 Claude Code 스킬 디렉터리에 배치

조직 단위 스킬:
- 관리자가 작업공간 전체에 스킬 배포 가능(2025-12-18 출시)
- 자동 업데이트
- 중앙 집중 관리

## 개방형 표준

Agent Skills를 개방형 표준으로 공개했습니다. MCP와 마찬가지로, 스킬도 도구/플랫폼 간 이식 가능해야 한다고 봅니다. 즉, 같은 스킬이 Claude뿐 아니라 다른 AI 플랫폼에서도 동작해야 합니다. 다만 일부 스킬은 특정 플랫폼 기능을 최대 활용하도록 설계될 수 있으며, 이런 경우 `compatibility` 필드에 명시할 수 있습니다. 생태계 구성원들과 표준을 함께 발전시키고 있으며, 초기 채택 사례도 고무적입니다.

## API를 통한 스킬 사용

애플리케이션, 에이전트, 자동화 워크플로처럼 프로그래밍 방식 사용 사례에서는 API가 스킬 관리와 실행을 직접 제어할 수 있게 해줍니다.

### 핵심 기능
- 스킬 목록/관리를 위한 `/v1/skills` 엔드포인트
- Messages API 요청에 `container.skills` 파라미터로 스킬 첨부
- Claude Console을 통한 버전 관리
- 맞춤 에이전트 구축을 위한 Claude Agent SDK와 연동

### API 사용 vs Claude.ai 사용 기준

사용 사례 / 권장 표면
- 최종 사용자가 스킬을 직접 사용하는 경우: Claude.ai / Claude Code
- 개발 중 수동 테스트 및 반복 개선: Claude.ai / Claude Code
- 개인의 즉흥적 워크플로: Claude.ai / Claude Code
- 앱에서 스킬을 프로그래밍 방식으로 사용하는 경우: API
- 대규모 프로덕션 배포: API
- 자동화 파이프라인 및 에이전트 시스템: API

참고: API에서 스킬을 사용하려면 Code Execution Tool 베타가 필요합니다. 이 기능이 스킬 실행에 필요한 보안 환경을 제공합니다.

### 구현 상세 문서
- Skills API Quickstart
- Create Custom skills
- Skills in the Agent SDK

## 현재 권장 접근

먼저 GitHub 공개 저장소에 스킬을 호스팅하세요. 사람 독자를 위한 명확한 README(스킬 폴더 내부가 아닌 저장소 루트), 스크린샷이 포함된 사용 예시를 제공하세요. 그다음 MCP 문서에 스킬 링크 섹션을 추가해, 둘을 함께 쓸 때의 가치와 빠른 시작 가이드를 안내하세요.

1. GitHub에 호스팅
- 오픈소스 스킬용 공개 저장소 사용
- 설치 방법이 명확한 README 작성
- 사용 예시와 스크린샷 포함

2. MCP 저장소에 문서화
- MCP 문서에서 스킬 링크 제공
- 둘을 함께 쓰는 가치 설명
- 빠른 시작 가이드 제공

3. 설치 가이드 작성

```markdown
## Installing the [Your Service] skill

1. Download the skill:
   - Clone repo: `git clone https://github.com/yourcompany/skills`
   - Or download ZIP from Releases

2. Install in Claude:
   - Open Claude.ai > Settings > Skills
   - Click "Upload skill"
   - Select the skill folder (zipped)

3. Enable the skill:
   - Toggle on the [Your Service] skill
   - Ensure your MCP server is connected

4. Test:
   - Ask Claude: "Set up a new project in [Your Service]"
```

## 스킬 포지셔닝

사용자가 가치를 이해하고 실제로 시도하게 하려면, 스킬을 설명하는 방식이 매우 중요합니다. README, 문서, 마케팅 문구를 작성할 때 아래 원칙을 지키세요.

### 기능보다 성과를 강조하기

✅ Good:

"ProjectHub 스킬을 사용하면 팀이 페이지, 데이터베이스, 템플릿을 포함한 완전한 프로젝트 작업공간을 몇 초 만에 만들 수 있어, 수동 설정 30분을 줄일 수 있습니다."

❌ Bad:

"ProjectHub 스킬은 YAML 프런트매터와 마크다운 지침이 들어 있는 폴더이며 MCP 서버 도구를 호출합니다."

### MCP + 스킬 결합 스토리 강조

"우리 MCP 서버는 클로드가 Linear 프로젝트에 접근하게 해줍니다. 우리 스킬은 팀의 스프린트 계획 워크플로를 클로드에 가르칩니다. 둘을 함께 쓰면 AI 기반 프로젝트 관리가 가능해집니다."
