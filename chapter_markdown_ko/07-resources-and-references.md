# 자료와 참고문헌

첫 스킬을 만들고 있다면 먼저 Best Practices Guide부터 읽고, 필요할 때 API 문서를 참조하세요.

## 공식 문서

### Anthropic 자료
- Best Practices Guide
- Skills Documentation
- API Reference
- MCP Documentation

### 블로그 글
- Introducing Agent Skills
- Engineering Blog: Equipping Agents for the Real World
- Skills Explained
- How to Create Skills for Claude
- Building Skills for Claude Code
- Improving Frontend Design through Skills

## 예시 스킬

### 공개 스킬 저장소
- GitHub: `anthropics/skills`
- Anthropic이 만든 커스터마이즈 가능한 스킬 포함

## 도구와 유틸리티

### `skill-creator` 스킬
- Claude.ai에 내장되어 있으며 Claude Code에서도 사용 가능
- 설명만으로 스킬 초안 생성 가능
- 리뷰와 개선 권고 제공
- 사용 예: `Help me build a skill using skill-creator`

### 검증
- `skill-creator`로 스킬 상태를 평가 가능
- 요청 예: `Review this skill and suggest improvements`

## 지원 받기

### 기술 질문
- 일반 질문: Claude Developers Discord의 커뮤니티 포럼

### 버그 리포트
- GitHub Issues: `anthropics/skills/issues`
- 포함 항목: 스킬 이름, 오류 메시지, 재현 절차

## 참고 A: 빠른 체크리스트

업로드 전/후로 스킬을 검증할 때 사용하세요. 더 빠르게 시작하고 싶다면 `skill-creator`로 초안을 만든 뒤, 이 목록으로 누락 사항을 점검하세요.

### 시작 전
- 구체적인 사용 사례 2~3개를 정의했다
- 필요한 도구(내장 또는 MCP)를 식별했다
- 본 가이드와 예시 스킬을 검토했다
- 폴더 구조를 계획했다

### 개발 중
- 폴더명이 케밥 케이스다
- `SKILL.md` 파일이 존재한다(정확한 철자)
- YAML 프런트매터에 `---` 구분자가 있다
- `name` 필드는 케밥 케이스, 공백/대문자 없음
- `description`에 WHAT과 WHEN이 모두 포함된다
- XML 태그(`< >`)가 어디에도 없다
- 지침이 명확하고 실행 가능하다
- 오류 처리를 포함했다
- 예시를 제공했다
- 참고 문서 링크가 명확하다

### 업로드 전
- 명백한 작업에서 트리거 테스트 완료
- 바꿔 말한 요청에서도 트리거 테스트 완료
- 무관한 주제에서 트리거되지 않음을 확인
- 기능 테스트 통과
- 도구 통합 동작 확인(해당 시)
- `.zip` 파일로 압축 완료

### 업로드 후
- 실제 대화에서 테스트
- 과소/과다 트리거 모니터링
- 사용자 피드백 수집
- 설명과 지침 반복 개선
- `metadata`의 버전 갱신

## 참고 B: YAML 프런트매터

### 필수 필드

```yaml
---
name: skill-name-in-kebab-case
description: What it does and when to use it. Include specific trigger phrases.
---
```

### 선택 필드 포함 전체 예시

```yaml
name: skill-name
description: [required description]
license: MIT # Optional: License for open-source
allowed-tools: "Bash(python:*) Bash(npm:*) WebFetch" # Optional: Restrict tool access
metadata: # Optional: Custom fields
  author: Company Name
  version: 1.0.0
  mcp-server: server-name
  category: productivity
  tags: [project-management, automation]
  documentation: https://example.com/docs
  support: support@example.com
```

### 보안 참고

#### 허용
- 표준 YAML 타입(문자열, 숫자, 불리언, 리스트, 객체)
- 커스텀 `metadata` 필드
- 긴 설명(최대 1024자)

#### 금지
- XML 꺾쇠 괄호(`< >`): 보안 제한
- YAML 내 코드 실행(안전한 YAML 파싱 사용)
- 이름 접두어가 `claude` 또는 `anthropic`인 스킬(예약)

## 참고 C: 완전한 스킬 예시

이 가이드의 패턴을 실제 프로덕션 수준으로 보여주는 전체 예시는 다음을 참고하세요.
- Document Skills: PDF, DOCX, PPTX, XLSX 생성
- Example Skills: 다양한 워크플로 패턴
- Partner Skills Directory: Asana, Atlassian, Canva, Figma, Sentry, Zapier 등 파트너 스킬

이 저장소들은 지속적으로 최신 상태를 유지하며, 본문에서 다루지 못한 추가 예시도 제공합니다. 저장소를 복제해 사용 사례에 맞게 수정하고, 템플릿으로 활용하세요.

`claude.ai`
