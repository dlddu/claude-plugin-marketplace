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

# CI 상태 확인 (스크립트 사용)
./scripts/check-ci.sh

# CI 상태 수동 확인
gh run list --limit 5

# 린트 실행
[프로젝트별 린트 명령어]
```

### 8. CI 검증 스크립트

CI 통과 여부 확인 시 아래 스크립트를 `scripts/check-ci.sh`로 저장하여 사용합니다:

```bash
#!/bin/bash
# CI 상태 확인 스크립트

set -e

MAX_WAIT_TIME=600  # 최대 10분 대기
CHECK_INTERVAL=30  # 30초 간격으로 확인

# 색상 정의
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

log_info() { echo -e "${GREEN}[INFO]${NC} $1" >&2; }
log_warn() { echo -e "${YELLOW}[WARN]${NC} $1" >&2; }
log_error() { echo -e "${RED}[ERROR]${NC} $1" >&2; }

# 저장소 정보 가져오기
get_repo_info() {
    if [ -n "$GITHUB_REPOSITORY" ]; then
        echo "$GITHUB_REPOSITORY"
        return 0
    fi
    local remote_url=$(git remote get-url origin 2>/dev/null)
    if [[ "$remote_url" =~ github\.com[:/]([^/]+)/([^/.]+) ]]; then
        echo "${BASH_REMATCH[1]}/${BASH_REMATCH[2]}"
        return 0
    fi
    if [[ "$remote_url" =~ /git/([^/]+)/([^/.]+) ]]; then
        echo "${BASH_REMATCH[1]}/${BASH_REMATCH[2]}"
        return 0
    fi
    return 1
}

# gh CLI 설치
install_gh_cli() {
    if command -v gh &> /dev/null; then
        log_info "GitHub CLI already installed"
        return 0
    fi
    log_info "Installing GitHub CLI..."
    local VERSION="2.63.2"
    local ARCH=$(uname -m)
    local OS=$(uname -s | tr '[:upper:]' '[:lower:]')
    case "$ARCH" in
        x86_64) ARCH="amd64" ;;
        aarch64|arm64) ARCH="arm64" ;;
    esac
    local URL="https://github.com/cli/cli/releases/download/v${VERSION}/gh_${VERSION}_${OS}_${ARCH}.tar.gz"
    local TMP_DIR=$(mktemp -d)
    curl -sL "$URL" | tar xz -C "$TMP_DIR"
    sudo mv "$TMP_DIR/gh_${VERSION}_${OS}_${ARCH}/bin/gh" /usr/local/bin/
    rm -rf "$TMP_DIR"
}

# gh CLI 인증
setup_gh_auth() {
    if gh auth status &> /dev/null; then return 0; fi
    if [ -n "$GITHUB_TOKEN" ]; then
        echo "$GITHUB_TOKEN" | gh auth login --with-token
        return 0
    fi
    if [ -n "$GH_TOKEN" ]; then return 0; fi
    log_error "No authentication token found. Set GITHUB_TOKEN or GH_TOKEN."
    exit 1
}

# 모든 워크플로우 완료 대기
wait_for_all_workflows() {
    local repo=$1 commit_sha=$2 elapsed=0
    local token="${GITHUB_TOKEN:-$GH_TOKEN}"

    log_info "Waiting for all workflows for commit $commit_sha..."

    while [ $elapsed -lt $MAX_WAIT_TIME ]; do
        local runs_response=$(curl -s -H "Authorization: token $token" \
            -H "Accept: application/vnd.github.v3+json" \
            "https://api.github.com/repos/${repo}/actions/runs?head_sha=${commit_sha}&per_page=100")

        local total_count=$(echo "$runs_response" | jq -r '.total_count')
        if [ "$total_count" = "0" ] || [ "$total_count" = "null" ]; then
            log_warn "No workflows found yet... (elapsed: ${elapsed}s)"
            sleep $CHECK_INTERVAL
            elapsed=$((elapsed + CHECK_INTERVAL))
            continue
        fi

        local all_completed=true any_failed=false
        while IFS= read -r run; do
            local status=$(echo "$run" | jq -r '.status')
            local conclusion=$(echo "$run" | jq -r '.conclusion')
            [ "$status" != "completed" ] && all_completed=false
            [ "$conclusion" = "failure" ] && any_failed=true
        done < <(echo "$runs_response" | jq -c '.workflow_runs[]')

        if [ "$all_completed" = true ]; then
            [ "$any_failed" = true ] && { echo "failure"; return 1; }
            echo "success"; return 0
        fi

        sleep $CHECK_INTERVAL
        elapsed=$((elapsed + CHECK_INTERVAL))
    done
    echo "timeout"; return 2
}

# 메인
main() {
    local repo=$(get_repo_info)
    local commit_sha=$(git rev-parse HEAD)

    [ -z "$repo" ] && { log_error "Could not determine repository"; exit 1; }

    install_gh_cli
    setup_gh_auth

    log_info "Repository: $repo, Commit: $commit_sha"
    sleep 10  # 워크플로우 시작 대기

    local result=$(wait_for_all_workflows "$repo" "$commit_sha")
    case "$result" in
        "success") log_info "✅ All workflows passed!"; exit 0 ;;
        "failure") log_error "❌ One or more workflows failed!"; exit 1 ;;
        "timeout") log_error "⏰ Timeout"; exit 2 ;;
    esac
}

main "$@"
```

**사용법:**
```bash
chmod +x scripts/check-ci.sh
./scripts/check-ci.sh
```

**환경 변수 설정:**
```bash
export GITHUB_TOKEN="your-token"
# 또는
export GH_TOKEN="your-token"
```

**종료 코드:**
- `0`: 모든 워크플로우 성공
- `1`: 하나 이상의 워크플로우 실패
- `2`: 타임아웃

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
