---
name: session-briefing
description: "세션 브리핑 자동화. 작업 완료시 결정사항·미결·다음액션을 볼트 Agent-Ops/_session-briefs/에 저장 + _latest.md 동기. 세션 종료·작업 완료·15턴↑·'이어서' 시 자동발동. 로드는 _latest.md 1회 read. 세션브리핑, session briefing, 이어서, 작업 완료, 세션 저장 맥락에서 사용. NOT: UP수정(→up-manager), 산출물저장."
---

<!-- Trigger Conditions
P1: 세션브리핑, 세션저장, session briefing, 이어서, 작업완료.
P2: 브리핑해줘, 저장해줘, 이어서, resume, brief, save session.
P3: session brief, context preservation, session continuity.
P4: 작업 완료 후 자동, 15턴 이상 도달시, 새 세션 시작시 _latest.md 자동 로드.
P5: .md로, _session-briefs/에.
NOT: UP수정(→up-manager), 산출물저장, 일반문서작성(→직접수행).
-->

# Session Briefing

세션 간 컨텍스트 연속성을 보장하는 스킬. 작업 결과를 고정 경로에 저장하고, 다음 세션에서 자동 로드.

---

## 발동 조건

| 조건 | 행동 |
|------|------|
| 작업 완료 | 자동실행 (제안 아님) |
| 15턴↑ 도달 | 자동실행 + 1줄 보고 |
| 형 "이어서" | `_latest.md` 1회 read → 작업 재개 |
| 새 세션 시작 | `_latest.md` 1회 read → 자동 로드 |
| 형 "생략" | 1회 스킵 |

---

## 경로 · 네이밍

```
VAULT  = CLAUDE.md §MOUNT 의 VAULT 경로
PATH   = {VAULT}/Agent-Ops/_session-briefs/YYMMDD-HHmm_[태그]_작업명.md
LATEST = {VAULT}/Agent-Ops/_session-briefs/_latest.md
```

### TIME_ACQUIRE
YYMMDD = env date에서 추출. HHmm = bash `date +%H%M` 1회 호출. 파일명: `YYMMDD-HHmm_[태그]_작업명.md`.

이유: bash date 1회(+2~3초)는 같은 날 복수 세션 구분을 위해 허용. HHmm 없으면 작업명이 유사할 때 덮어쓰기 사고 발생.

### 네이밍 규칙
- 태그: 프로젝트 폴더명 축약 (예: "C8BP")
- 프로젝트 미확정 → 태그 생략

---

## 콘텐츠

3블록 (순서 고정). 각 항목마다 관련 파일 경로 `→ path` 허용 (블록 제한 없음).

1. **결정사항** — 이번 세션에서 확정된 것 + `→ 관련 파일`
2. **미결** — 보류 중인 것 + 사유 + 외부 대기 항목 + `→ 관련 파일`
3. **다음액션** — 다음 세션 할 일 + `→ 관련 파일`

이유: "이어서" 재개 시 결정사항·미결에도 파일 경로가 있어야 맥락 복원 가능. 다음액션에만 포인터가 있으면 "왜 이렇게 결정했지?"를 역추적 불가.

### _latest.md 동기

**_latest.md 정합성 검증:** 로드 시 _latest.md의 mtime과 해당 날짜 브리핑 파일의 mtime 비교. 불일치(날짜 브리핑이 더 신규) → _latest.md 재동기화 후 로드. 일치 → 그대로 사용.

매 저장 시 본 브리핑 내용을 `_latest.md`에도 동일하게 쓴다 (2회 저장).

이유: 로드 시 list_directory→정렬→read 3단계를 read 1회로 단축. 저장 시 +1회 쓰기는 로드 시 -2회 읽기로 상쇄.

---

## 문서화 전환 게이트

산출물 작성 완료 후 → 브리핑 전에 문서화 전환 제안 가능:

| 조건 | 행동 |
|------|------|
| 합의 3조건 ALL (❶핵심방향 명시적 동의 ❷수정요청 없음 ❸재검토 신호 없음) | "문서로 옮길까요?" 제안 |
| "써줘/저장해" | 즉시 실행 |
| spine 미합의 | 문서화 ✗ |

---

## 브리핑 없이 종료 = 컨텍스트 소실

이유: Claude의 세션 간 메모리는 세션 브리핑이 유일한 장치. 저장하지 않으면 다음 세션에서 처음부터 시작해야 한다.

---

## 속도 규칙

| 규칙 | 이유 |
|------|------|
| bash `date +%H%M` 1회 허용 — YYMMDD는 env date, HHmm은 bash | 같은 날 복수 세션 구분 (+2~3초 허용) |
| 디렉토리 존재 확인 생략 — 쓰기 실패 시에만 create | MCP 호출 1회 제거 (-2초) |
| 같은 날 재저장 = 덮어쓰기 (접미사 `_2` 불사용) | list_directory 호출 제거 (-2초) |
| 저장 = 2회 (본파일 + _latest.md) | 로드 시 -2회 읽기로 상쇄 |
| 로드 = `_latest.md` 1회 read | 정렬·검색 불필요 |

---

## Gotchas

- **프로젝트 태그 과잉:** "C8-Business-Plan-Meeting-2026-04" 대신 "C8BP" 수준으로 축약
- **브리핑 과다:** 일반 대화에서 매번 저장 → 노이즈. 발동 조건표 엄격 준수
- **볼트 접근 확인:** 저장 전 볼트 경로 접근 가능 여부 확인. 불가 시 마운트 등 대안 탐색
- **_latest.md 갱신 누락:** 본파일만 쓰고 _latest.md 안 쓰면 로드 단축 무효화
- **컨텍스트 포인터 누락:** 결정사항·미결에도 `→ path` 필수. 다음액션에만 넣으면 재개 시 역추적 불가
- (실수 발견 시 직접 추가)
