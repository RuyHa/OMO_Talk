# OMO Talk Patch Notes

## English

### 1.0.1 (from v1.0.0)

- Reliability: stabilized tmux execution and server handling with a dedicated OMO Talk socket.
- Telegram relay: improved pending-reply detection/extraction so ACK-only and noisy outputs are less likely.
- Session path UX: added `/name` single-segment fallback to Base Path in both UI and Telegram commands.
- UI refresh: moved toward macOS HIG style (materials, typography, spacing) and improved Help readability/layout consistency.
- Docs: simplified Quick Start flow in the app/help and `omo_git/README.md`.
- Release packaging: default app version updated to `1.0.1` and DMG signing/notarization flow maintained.

### 1.0.0

- Initial release baseline.

---

## 한국어

### 1.0.1 (v1.0.0 대비)

- 안정성: tmux 실행/서버 처리 로직을 보강하고 OMO Talk 전용 소켓 기반으로 안정화했습니다.
- 텔레그램 릴레이: pending 응답 감지/추출 로직을 개선해 ACK만 오고 답장이 누락되는 케이스와 노이즈 유입을 줄였습니다.
- 세션 경로 UX: UI와 텔레그램 명령 모두에서 `/name` 단일 경로 입력 시 Base Path 보정 동작을 추가했습니다.
- UI 개편: macOS HIG 방향(머티리얼, 타이포, 간격)으로 정리하고 Help 화면 가독성/카드 정렬을 개선했습니다.
- 문서 정리: 앱/도움말 빠른 시작 흐름과 `omo_git/README.md` 설치 가이드를 더 간결하게 정리했습니다.
- 배포: 기본 앱 버전을 `1.0.1`로 올리고, DMG 서명/노타라이즈 배포 흐름을 유지했습니다.

### 1.0.0

- 초기 공개 버전.
