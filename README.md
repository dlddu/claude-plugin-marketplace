# claude-plugin-marketplace

dlddu의 Claude Code 플러그인 마켓플레이스입니다.

## 설치 방법

### 1. 마켓플레이스 추가

```bash
/plugin marketplace add dlddu/claude-plugin-marketplace
```

### 2. 플러그인 설치

```bash
/plugin install sdd-workflow@dlddu-plugins
```

## 플러그인 목록

### sdd-workflow

SDD(Spec Driven Development) 자동화 워크플로우를 실행하는 skill입니다.

**기능:**
- 추적성 매트릭스 분석
- 계획서 자동 생성
- 구현 및 테스트
- Git commit & push
- CI 상태 자동 확인

**트리거 문구:**
- "다음 작업 진행해줘"
- "구현 시작해줘"
- "SDD 워크플로우 실행"

## 마켓플레이스 명령어

```bash
# 마켓플레이스 목록 보기
/plugin marketplace list

# 마켓플레이스 업데이트
/plugin marketplace update

# 마켓플레이스 제거
/plugin marketplace remove dlddu-plugins
```
