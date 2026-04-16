# organize-claude-md

`CLAUDE.md`를 **Lazy Loading 참조 구조** + **프레임워크 특화 템플릿** + **Mermaid 아키텍처 다이어그램**으로 자동 재구성하는 Claude Code 슬래시 커맨드입니다.

## 왜 필요한가

`CLAUDE.md`는 매 세션 시작 시 전체가 컨텍스트에 로드됩니다. 120줄을 넘기면 에이전트가 핵심 지시를 놓치고 compliance가 떨어집니다. 이 커맨드는:

- 메인 `CLAUDE.md`를 **120줄 이하**로 유지 (항상 로드되는 핵심만)
- 상세 문서를 `.claude/docs/reference/`로 분리 (필요할 때만 로드)
- 프레임워크 + 언어를 자동 감지하여 맞춤 아키텍처 다이어그램 생성
- 금지형 규칙("NEVER do X")을 우선 사용 — 권장형보다 compliance가 높음

## 지원 프레임워크

| 카테고리 | 프레임워크 |
|----------|-----------|
| JVM | Spring Boot (Java), Spring Boot (Kotlin), Quarkus, Micronaut |
| JS/TS 프론트엔드 | React, Next.js, Vue, Nuxt, Angular, Svelte/SvelteKit |
| JS/TS 백엔드 | NestJS, Express, Fastify |
| 모바일 | Flutter (Dart), React Native |
| Python | Django, FastAPI, Flask |
| 기타 | Go (Gin/Echo/Fiber), Rust (Actix/Axum/Rocket) |

## 설치

커맨드 파일을 Claude Code commands 디렉토리에 복사하세요:

```bash
# 글로벌 (모든 프로젝트에서 사용 가능)
cp organize-claude-md.md ~/.claude/commands/

# 프로젝트 로컬 (해당 프로젝트에서만 사용 가능)
cp organize-claude-md.md .claude/commands/
```

## 사용법

Claude Code에서 실행:

```
/organize-claude-md
```

### 인자

| 인자 | 설명 |
|------|------|
| _(빈 값)_ | 전체 재구성 (CLAUDE.md + 참조 문서) |
| `main` | CLAUDE.md만 재구성 |
| `references` | 참조 문서만 생성/업데이트 |
| `module:<name>` | 특정 모듈의 참조 문서만 (예: `module:repair`) |
| `scan` | 현황 분석만 (변경 없이 리포트) |
| `diff` | 기존 CLAUDE.md vs 제안 변경사항 비교 |

## 동작 과정

### Phase 1 — 프로젝트 분석
- 기존 CLAUDE.md와 참조 문서 확인
- 프레임워크 + 언어 자동 감지 (예: `Spring Boot 3.3.8 (Java 21)`)
- 실제 코드 패턴을 딥 스캔 (추측 금지)

### Phase 2 — 콘텐츠 분류
- "항상 로드" (메인) vs "온디맨드" (참조 문서)로 분류
- Skills 섹션은 반드시 메인에 유지

### Phase 3 — 아키텍처 다이어그램
- 실제 코드 구조를 기반으로 Mermaid 다이어그램 생성
- 프레임워크별 맞춤 템플릿 (Spring Boot, React, Vue, Flutter, NestJS)

### Phase 4 — 메인 CLAUDE.md 작성
- 120줄 이하로 재작성
- 금지형 규칙 우선 적용
- 참조 문서 인덱스 테이블 포함

### Phase 5 — 참조 문서 생성
- `.claude/docs/reference/`에 문서 생성
- 기존 문서는 덮어쓰지 않고 병합
- 문서당 200줄 이하

### Phase 6~7 — 확인 및 검증
- 변경 계획을 보여주고 사용자 확인 후 실행
- 줄 수, 파일 경로, 정보 보존 여부 검증

## 핵심 원칙

- **절대 삭제 안 함** — 기존 CLAUDE.md 정보는 메인 또는 참조 문서로 반드시 이동
- **절대 덮어쓰기 안 함** — 기존 참조 문서와 병합
- **언어 정확성** — Java 프로젝트에는 Java 패턴, Kotlin에는 Kotlin 패턴 (혼용 불가)
- **코드 기반** — 모든 패턴은 실제 프로젝트 코드에서 추출 (일반 템플릿 사용 안 함)

## 라이선스

MIT
