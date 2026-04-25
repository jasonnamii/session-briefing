# session-briefing — CHANGELOG

## v39 (2026-04-25)
- **이원화 저장 구조**: 프로젝트 단서 있음=프로젝트 폴더, 없음=글로벌 `{VAULT}/_session_briefing/` 자동 폴백
- **질문 턴 제거**: PROJECT_ROOT 미확정 시 1줄 질문 → 글로벌 폴백으로 대체 (속도 최대화)
- **VAULT resolve 속도화**: 세션캐시 키 `VAULT_RESOLVED` 도입, CLAUDE.md read 세션 1회로 고정
- **폴더명 공백 제거**: `_session briefing/` → `_session_briefing/` (bash 따옴표 불필요)
- **저장 2회 병렬**: 본파일+`_latest.md` 동시 write
- **절대경로 금지 강화**: 절대규칙 #1로 승격, self-check에 "본문 내 절대경로 0개" 추가
- **rationale.md 포인터 제거**: 누락 파일 참조 깨짐 해결, Gotchas 본문에 인라인화
- description에 "세션핸드오프" P1 추가

## autoloop-v1 (2026-04-23)
- description 표준화: P1~NOT 통합, frontmatter 단일라인화
- ⛔ 절대 규칙 (INVARIANT) 섹션 신설
- 예시 섹션 추가
- Gotchas 표 형식 전환/확장
- self-check 언급 추가
- evals/cases.json 신설
- skill-doctor 점수: 기존 대비 평균 +17.7점 향상
