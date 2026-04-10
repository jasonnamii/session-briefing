# 세션 브리핑

**Claude 세션 간 맥락 연속성을 위한 세션 브리핑 자동화 도구.**

> 🇺🇸 [English README](./README.md)

## 사전 요구사항

- **Obsidian Vault** — `Agent-Ops/_session-briefs/` 세션 브리핑 저장소로 사용
- **Desktop Commander MCP** — Vault 파일 쓰기 필요 (Obsidian MCP는 읽기 전용)
- **Claude Cowork 또는 Claude Code** 환경

## 목적

session-briefing은 세션 간 작업 손실을 방지합니다. 의사결정, 미결 사항, 다음 액션을 타임스탬프와 함께 자동으로 캡처하여 원활한 작업 연속성을 보장합니다.

## 사용 시점 및 방법

자동 발동: 세션 종료, 작업 완료, 15턴 이상, 또는 '이어서' 명령. Agent-Ops/_session-briefs/에 타임스탬프 파일명으로 저장됩니다.

## 사용 예시

| 상황 | 프롬프트 | 결과 |
|---|---|---|
| 세션 종료 | (자동 발동) | 의사결정, 미결 사항, 다음 액션 추출→브리핑 저장→다음 세션 준비 완료 |
| 작업 완료 | (자동 발동) | 산출물, 블로커, 남은 작업 캡처→브리핑 저장 |
| 이어서 작업 | `"이 프로젝트 이어서."` | 최신 브리핑 읽음→모든 맥락 로드→설정 반복 없이 재개 |

## 핵심 기능

- 세션 경계와 마일스톤에서 자동 캡처
- 4가지 발동 지점: 세션 종료, 작업 완료, 15턴 이상, '이어서'
- 타임스탬프 기반 시계열 저장
- 구조화된 형식: 의사결정, 미결 사항, 다음 액션
- 원활한 계속 작업을 위한 맥락 복원

## 연관 스킬

- **[up-manager](https://github.com/jasonnamii/up-manager)** — 맥락 추적을 위해 UP 변경 기록
- **[deliverable-engine](https://github.com/jasonnamii/deliverable-engine)** — 산출물 상태 추적
## 설치

```bash
git clone https://github.com/jasonnamii/session-briefing.git ~/.claude/skills/session-briefing
```

## 업데이트

```bash
cd ~/.claude/skills/session-briefing && git pull
```

`~/.claude/skills/`에 배치된 스킬은 Claude Code 및 Cowork 세션에서 자동으로 사용할 수 있습니다.

## Cowork 스킬 생태계

25개 이상의 커스텀 스킬 중 하나입니다. 전체 카탈로그: [github.com/jasonnamii/cowork-skills](https://github.com/jasonnamii/cowork-skills)

## 라이선스

MIT 라이선스 — 자유롭게 사용, 수정, 공유하세요.
