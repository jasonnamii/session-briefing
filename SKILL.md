---
name: session-briefing
description: "세션 브리핑 자동화 v38.1. 작업 완료시 결정·미결·다음액션을 볼트 내 프로젝트 폴더 _session briefing/에 저장 + _latest.md 동기. 터보(기본)=3줄요약, 풀모드=3블록+포인터. VAULT resolve·PROJECT_ROOT 세션캐시·no-op가드 내장. P1: 세션브리핑, 세션저장, session briefing, 이어서, 작업완료, 브리핑, 세션저장해줘. P2: 브리핑해줘, 저장해줘, 이어서, resume, brief, save session. P3: session brief, context preservation, session continuity, session handoff. P5: md파일로, 프로젝트 폴더 _session briefing/ 저장. NOT: UP수정(→up-manager), 산출물저장, 일반문서작성(→직접수행)."
vault_dependency: SOFT
---

# Session Briefing

세션 간 컨텍스트 연속성 보장. 작업 결과를 볼트 고정 경로에 저장, 다음 세션에서 자동 로드.

---

## ⛔ 절대 규칙

| # | 규칙 | 이유 |
|---|------|------|
| 1 | **VAULT 하드코딩 금지** — 경로는 글로벌 CLAUDE.md `MOUNT::=VAULT`에서만 resolve | 머신·사용자 변경 시 즉사 방지 |
| 2 | **세션 마운트 경로 저장 금지** — 반드시 VAULT 하위 경로만 | 세션 종료 시 소실 방지 |
| 3 | **_latest.md 덮어쓰기 — append 금지** — 날짜별 사본이 과거 담당 | 정합성 보장 |
| 4 | **PROJECT_ROOT 미확정 시 저장 금지** — 1줄 질문으로 확정 후 진행 | 오저장 방지 |

### 자체 점검 (self-check)

스킬 수정 후: SKILL.md ≤5KB · P1 ≥5개 · 절대규칙 존재 · Gotchas ≥3행 확인.

---

## 발동

| 조건 | 모드 | 행동 |
|---|---|---|
| 작업 완료 | **터보** | 자동. 3줄 요약 + 2회 저장 (본파일·_latest.md 덮어쓰기) |
| 10턴↑ 도달 | **터보 + no-op 가드** | 결정·미결·다음 중 ≥1 변화시만 저장. 무변화 = 인-컨텍스트 1문단 자가요약 + 1줄 보고 |
| "이어서" / 새 세션 | 로드 | `_latest.md` **1회 read, 끝** (정합성 검증 ✗) |
| "풀 브리핑"·"상세" | **풀** | 3블록 + 포인터 전부 + 2회 저장 |
| "생략" | — | 1회 스킵 |

---

## 경로

```
VAULT        = 글로벌 CLAUDE.md `MOUNT ::= VAULT` resolve (세션 시작 1회, 캐시)
PROJECT_ROOT = {VAULT} 하위 프로젝트 폴더 (세션 캐시)
PATH         = {PROJECT_ROOT}/_session briefing/YYMMDD_작업명.md
LATEST       = {PROJECT_ROOT}/_session briefing/_latest.md
```

### PROJECT_ROOT 판정 (2단계)
1. **세션 캐시 hit** → 즉시 사용
2. **miss** → 1줄 질문으로 볼트 내 프로젝트 확정

프로젝트 전환은 사용자 명시 시에만 캐시 갱신. 세션 마운트·볼트 외부 폴백 ✗.

### 파일명
`YYMMDD_작업명.md` — YYMMDD는 env date. 같은 날 동일 작업명 = 덮어쓰기. `_session briefing` 폴더 공백 포함(bash 따옴표 필수), 없으면 쓰기 시 자동 생성.

---

## 콘텐츠

### 터보 (기본) — 3줄
```
- 결정: [확정된 핵심 1~2개 / 한 줄]
- 미결: [보류 + 사유 / 한 줄. 없으면 "없음"]
- 다음: [다음 세션 첫 행동 + → 파일경로 / 한 줄]
```
포인터(`→ path`)는 **다음액션에만 필수**. 결정·미결은 변경 파일 있을 때만.

### 풀 (명시 요청) — 3블록 + 포인터
1. **결정사항** + `→ 관련 파일`
2. **미결** + 사유 + 외부 대기 + `→ 관련 파일`
3. **다음액션** + `→ 관련 파일`

### 저장 = 2회
날짜별 사본 + `_latest.md` 둘 다 덮어쓰기. 누적 ✗. 터보에서도 날짜별 사본 필수(역추적 앵커).

---

## 문서화 전환 게이트

| 모드 | 동작 |
|---|---|
| 터보 (기본) | 게이트 **스킵** |
| 풀 | 3조건(핵심방향 동의·수정요청 없음·재검토 신호 없음) ALL → "문서로 옮길까요?" |
| "써줘/저장해" | 즉시 실행 |

---

## 속도 규칙

| 규칙 | 효과 |
|---|---|
| 터보 기본 (3줄) | 추론 토큰 -60~70% |
| PROJECT_ROOT + VAULT 세션 캐시 | 판정 -90%, VAULT resolve 1회만 |
| 10턴 no-op 가드 | 저장 -20~40% |
| 터보 시 게이트 스킵 | 대화 턴 -1~2 |
| 로드 = _latest.md 1회 read (검증 ✗) | I/O 단일화 |
| YYMMDD = env date | bash 호출 -1 |
| 디렉토리 존재 확인 생략 (쓰기 실패 시 create) | MCP 호출 -1 |
| 같은 날 덮어쓰기 | list 호출 -1 |

---

## 예시

| 상황 | 발동 | 결과 |
|---|---|---|
| 작업 완료 후 | 자동 터보 | `결정/미결/다음` 3줄 → VAULT 저장 |
| "이어서" 입력 | 로드 | `_latest.md` 1회 read → 컨텍스트 복원 |
| "풀 브리핑" 요청 | 풀 모드 | 3블록 + 포인터 전부 출력 + 저장 |
| 10턴 도달, 변화 없음 | no-op 가드 | 저장 스킵 → 인-컨텍스트 1문단 자가요약 |

---

## 상세

설계 근거·자기모순 방지 규칙: `→ references/rationale.md`

---

## Gotchas

| 함정 | 대응 |
|---|---|
| `_latest.md`에 append | 반드시 덮어쓰기. 과거는 날짜별 사본 담당 |
| 세션 마운트 경로에 저장 | VAULT 하위만. 판정 불가 시 1줄 질문 |
| VAULT 경로 하드코딩 | 글로벌 CLAUDE.md `MOUNT::=VAULT`가 유일 진실원천 |
| PROJECT_ROOT 미확정 저장 | 1줄 질문으로 확정 후 진행. 볼트 루트 폴백 ✗ |
| no-op 가드 오판 | 표현만 다르고 실질 변화 없으면 스킵 |

전수 Gotchas (12항): `→ references/rationale.md §Gotchas`
