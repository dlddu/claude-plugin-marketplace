---
name: generate-claude-md
description: >
  프로젝트의 CLAUDE.md 파일을 생성합니다.
  Agile-SDD-TDD 워크플로우를 Claude Code 에이전트가 이해할 수 있도록 정리합니다.
---

# CLAUDE.md 생성

프로젝트의 문서들을 분석하여 Claude Code 에이전트를 위한 `CLAUDE.md` 파일을 생성해주세요.

## 생성할 파일

프로젝트 루트에 `CLAUDE.md` 파일을 생성합니다.

## 포함해야 할 내용

### 1. 프로젝트 개요
- 프로젝트 목적과 범위
- 기술 스택

### 2. 개발 워크플로우

다음 워크플로우를 설명해주세요:

#### 명세 주도 개발 (SDD)
- 요구사항 → 명세서(SPEC.md)
- 명세서 → 사용자 스토리(USER-STORIES.md)
- 모든 스토리에는 인수 테스트 포함

#### 테스트 주도 개발 (TDD)
- Red: 실패하는 테스트 작성
- Green: 테스트 통과를 위한 최소 구현
- Refactor: 코드 품질 개선

#### 애자일 작업 관리
- 작업은 인수 테스트 기반으로 분할
- 작업 크기는 테스트 포함하여 판단
- 추적성 매트릭스로 진행 상황 관리

### 3. 문서 구조

```
docs/
├── SPEC.md                    # 요구사항 명세서
├── USER-STORIES.md            # 사용자 스토리 & 인수 기준
├── ARCHITECTURE.md            # 아키텍처 설계서
├── CI-PIPELINE.md             # CI 파이프라인 설계서
├── TRACEABILITY-MATRIX.md     # 추적성 매트릭스
└── plans/
    └── PLAN-XXX.md            # 작업 계획서
```

### 4. ID 체계

| 접두사 | 용도 | 예시 |
|--------|------|------|
| REQ-XXX | 요구사항 | REQ-001 |
| US-XXX | 사용자 스토리 | US-001 |
| AC-XXX-X | 인수 기준 | AC-001-1 |
| COMP-XXX | 컴포넌트 | COMP-001 |
| TASK-XXX | 작업 | TASK-001 |
| PLAN-XXX | 계획서 | PLAN-001 |

### 5. 스킬 사용 가이드

| 상황 | 사용할 스킬 |
|------|------------|
| 새 기능 요구사항 정리 | spec-writing |
| 프로젝트 초기 설정 | project-bootstrap |
| 다음 작업 결정 | task-planning |
| 코드 구현 | implementation |

### 6. 핵심 규칙

1. **사용자 스토리의 사용자 = 최종 사용자**
2. **모든 스토리에는 인수 테스트 필수**
3. **구현과 테스트는 항상 동시에**
4. **테스트 실패 시 명세 먼저 확인**
5. **CI 통과가 완료의 기준**
6. **추적성 매트릭스로 모든 것을 추적**

### 7. 명령어

```bash
# 테스트 실행
[프로젝트별 테스트 명령어]

# CI 상태 확인
gh run list --limit 5

# 린트 실행
[프로젝트별 린트 명령어]
```

## 생성 절차

1. 다음 문서들을 확인:
   - `docs/SPEC.md`
   - `docs/USER-STORIES.md`
   - `docs/ARCHITECTURE.md`
   - `package.json` 또는 프로젝트 설정 파일

2. 프로젝트에 맞게 CLAUDE.md 내용 커스터마이즈

3. 프로젝트 루트에 `CLAUDE.md` 파일 생성

## 참고

생성된 CLAUDE.md 파일은 Claude Code 에이전트가 프로젝트 컨텍스트를 빠르게 이해하고, 올바른 워크플로우를 따르도록 돕습니다.
