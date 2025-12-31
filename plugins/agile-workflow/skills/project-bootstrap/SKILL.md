---
name: project-bootstrap
description: >
  요구사항 명세에 기반하여 프로젝트를 부트스트랩합니다.
  아키텍처 설계, CI 파이프라인 구성, GitHub Actions 워크플로우 설정까지 수행합니다.
  "프로젝트 시작해줘", "부트스트랩 해줘", "초기 설정해줘" 등을 요청할 때 활성화됩니다.
---

# 프로젝트 부트스트랩 스킬 (Project Bootstrap Skill)

## 개요

이 스킬은 요구사항 명세를 기반으로 프로젝트의 초기 구조를 설정합니다:
1. **아키텍처 설계** (ARCHITECTURE.md)
2. **CI 파이프라인 설계** (CI-PIPELINE.md)
3. **GitHub Actions 워크플로우** (.github/workflows/)
4. **Hello World & Health Check 구현**
5. **추적성 매트릭스 업데이트**

## 사전 조건

- `docs/SPEC.md` 파일이 존재해야 합니다
- `docs/USER-STORIES.md` 파일이 존재해야 합니다

## 워크플로우

### Phase 1: 아키텍처 설계

`docs/ARCHITECTURE.md` 파일 생성:

```markdown
# 아키텍처 설계서

## 1. 시스템 개요
### 1.1 아키텍처 스타일
- [선택한 아키텍처 패턴: 레이어드/헥사고날/마이크로서비스 등]

### 1.2 기술 스택
- **언어**:
- **프레임워크**:
- **데이터베이스**:
- **기타**:

## 2. 시스템 구조
### 2.1 컴포넌트 다이어그램
[컴포넌트 간 관계 설명]

### 2.2 디렉토리 구조
```
project-root/
├── src/
│   ├── domain/          # 도메인 로직
│   ├── application/     # 애플리케이션 서비스
│   ├── infrastructure/  # 인프라 계층
│   └── presentation/    # 프레젠테이션 계층
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
└── .github/workflows/
```

## 3. 컴포넌트 명세
### COMP-001: [컴포넌트 이름]
- **책임**:
- **의존성**:
- **관련 요구사항**: REQ-XXX

## 4. 데이터 설계
## 5. API 설계
## 6. 보안 고려사항
## 7. 확장성 고려사항
```

### Phase 2: CI 파이프라인 설계

`docs/CI-PIPELINE.md` 파일 생성:

```markdown
# CI 파이프라인 설계서

## 1. 파이프라인 개요
- **CI 도구**: GitHub Actions
- **트리거 조건**: push, pull_request

## 2. 파이프라인 단계

### Stage 1: 빌드 (Build)
- 의존성 설치
- 컴파일/빌드

### Stage 2: 테스트 (Test)
- 단위 테스트 실행
- 통합 테스트 실행
- 커버리지 리포트 생성

### Stage 3: 정적 분석 (Lint)
- 코드 스타일 검사
- 보안 취약점 스캔

### Stage 4: 배포 (Deploy) - Optional
- 스테이징 배포
- 프로덕션 배포

## 3. 워크플로우 파일 목록
| 파일명 | 목적 | 트리거 |
|--------|------|--------|
| ci.yml | 메인 CI 파이프라인 | push, PR |
| deploy.yml | 배포 파이프라인 | release |

## 4. 환경 변수
## 5. 시크릿 관리
```

### Phase 3: GitHub Actions 워크플로우 생성

`.github/workflows/ci.yml` 파일 생성:

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup [언어/런타임]
        uses: actions/setup-[언어]@v[버전]
        with:
          [언어]-version: '[버전]'

      - name: Install dependencies
        run: [패키지 매니저 설치 명령]

      - name: Build
        run: [빌드 명령]

      - name: Run tests
        run: [테스트 명령]

      - name: Lint
        run: [린트 명령]

  health-check:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Health check
        run: echo "Health check passed"
```

### Phase 4: Hello World & Health Check 구현

기본 애플리케이션 진입점과 헬스체크 엔드포인트를 구현합니다:

#### 구현 체크리스트
- [ ] Hello World 응답 구현
- [ ] `/health` 또는 `/healthz` 엔드포인트 구현
- [ ] 기본 테스트 작성
- [ ] CI에서 테스트 통과 확인

#### 헬스체크 응답 형식
```json
{
  "status": "healthy",
  "timestamp": "2024-01-01T00:00:00Z",
  "version": "1.0.0"
}
```

### Phase 5: 추적성 매트릭스 업데이트

`docs/TRACEABILITY-MATRIX.md` 업데이트:

| 요구사항 ID | 사용자 스토리 | 컴포넌트 | 인수 테스트 | 구현 상태 | 테스트 상태 |
|------------|--------------|----------|------------|----------|------------|
| REQ-001 | US-001 | COMP-001 | AC-001-1 | ✅ | ✅ |

## 산출물 체크리스트

작업 완료 전 다음 항목을 확인하세요:

- [ ] `docs/ARCHITECTURE.md` 파일 생성됨
- [ ] `docs/CI-PIPELINE.md` 파일 생성됨
- [ ] `.github/workflows/ci.yml` 파일 생성됨
- [ ] Hello World 구현됨
- [ ] Health Check 엔드포인트 구현됨
- [ ] 기본 테스트 작성됨
- [ ] GitHub Actions 워크플로우 실행 성공
- [ ] 추적성 매트릭스 업데이트됨

## CI 검증

GitHub Actions 워크플로우가 성공적으로 실행되었는지 확인합니다:

```bash
# GitHub CLI를 사용한 워크플로우 상태 확인
gh run list --limit 5
gh run view [run-id]
```

## 사용 예시

```
사용자: 프로젝트 부트스트랩 해줘

Claude:
1. docs/SPEC.md 확인 중...
2. 아키텍처 설계 중...
3. CI 파이프라인 설계 중...
4. GitHub Actions 워크플로우 생성 중...
5. Hello World & Health Check 구현 중...
6. 테스트 작성 및 실행 중...
7. 추적성 매트릭스 업데이트 중...

부트스트랩 완료! CI 파이프라인이 성공적으로 실행되었습니다.
```

## 관련 스킬

- **spec-writing**: 명세 작성 (사전 필수)
- **task-planning**: 작업 계획 수립
- **implementation**: 기능 구현
