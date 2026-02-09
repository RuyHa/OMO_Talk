# OMO Talk

최근 OpenCode(opencode)와 Oh My OpenCode를 알게 되고 너무 잘 쓰고 있습니다.
좋은 툴들을 공유해주신 개발자 분들께 감사한 마음입니다.

해당 툴을 사용하면서 제가 필요했던 프로그램을 만들게 되었고,
혹시나 저와 비슷한 환경에서 작업하는 사용자에게 저 또한 도움이 되고 싶어
조금의 노력을 더해 앱으로 만들어 공유합니다.

앱스토어에 올리고 싶지만 다른 프로그램에 대한 의존성 문제들로 리젝이 될 것 같아,
Developer ID로 서명/노타라이즈한 DMG 형태로 공유합니다.

---

Recently I discovered OpenCode (opencode) and Oh My OpenCode, and they've become part of my daily workflow.
Huge thanks to the developers who built and shared these tools.

While using them, I ended up building a small helper app for my own needs.
I’m sharing it in case it helps others working in a similar setup.

I’d like to ship it on the App Store, but because it depends on external tools,
it would likely be rejected. So I’m distributing it as a Developer ID signed + notarized DMG.

Shoutouts:
- OpenCode (opencode): https://github.com/opencode-ai/opencode (@opencode-ai)
- Oh My OpenCode: https://github.com/code-yeongyu/oh-my-opencode (@code-yeongyu)

## English

This document is written so you can install, configure, and use **OMO Talk** by following this file only.

OMO Talk is a macOS menubar app that lets you continue work through the flow:

**Telegram message → tmux session (terminal) → opencode**

---

## 0) Prerequisites (short)

You need the following:

1) macOS 13+
2) iTerm2 installed
3) tmux installed (`tmux -V` works)
4) opencode + oh-my-opencode installed (`opencode --version` works)

---

## 1) Install (DMG)

1) Double-click `OMO Talk.dmg`.
2) Drag `OMO Talk.app` to `Applications`.
3) Launch `/Applications/OMO Talk.app`.

Version check:

- Finder -> `/Applications` -> `OMO Talk.app` -> `Command + I` -> Version

---

## 2) Quick Start (same as in-app)

1) Install opencode and oh-my-opencode.
2) Install tmux.
3) In Telegram, open `@BotFather`.
4) Create a bot with `/newbot` and copy the token.
5) In OMO Talk Settings, paste **Bot Token**.
6) Click **Save**.
7) Click **Start Bot** in the main panel.
8) Send any message to your bot.
9) Put the detected `user_id` into **Allowed User ID**, then save again.
10) Create a session with **Add Session**, or run `/new <alias> <path>` in Telegram.

Example:

- If your project is `/Users/user/Desktop/ppap` and Base Path is `/Users/user/Desktop`, you can use `/new ppap /ppap`.

---

## 3) Path input rule (`/new` and Add Session)

- Full absolute path: `/Users/you/...`
- Home path: `~/...`
- Relative path: resolved under Base Path (e.g. `project`)
- Convenience fallback: if Base Path is `/Users/you/Desktop`, `/new app /app` can be interpreted as `/Users/you/Desktop/app`

---

## 4) Use from Telegram

### 4-1) Basic usage (explicit alias/path)

Basic format:

```text
<alias|path> ~~ <prompt>
```

Example:

```text
app ~~ summarize the project structure
```

### 4-2) Per-chat default session (`/base`)

Telegram supports not only 1:1 chats with your bot but also inviting the bot into **group chats**.
This lets you map chats to sessions, for example:

- `Project A` group → tmux session A
- `Project B` group → tmux session B

If you set `/base` once per chat, you don't need to prefix every message with `alias ~~`.

In chats where you want to avoid typing `alias ~~` every time, set a default session.

1) In that chat:

```text
/base <alias|path>
```

Example:

```text
/base app
```

2) After that, plain messages route to the default session:

```text
plan today's tasks
```

3) Clear the default session:

```text
/base clear
```

### 4-3) Send to another session even when `/base` is set

Even if the chat default is `A`, you can send a single message to `B` like this:

```text
B ~~ send this to session B
```

---

## 5) Full command list

These are also available via `/help`.

- `/new <path>`
- `/new <alias> <path>`
- `/attach <alias|path>`
- `/list`
- `/status <alias|path>`
- `<alias|path> ~~ <prompt>`
- `/close <alias|path>`
- `/base <alias|path|clear>`
- `/lang en|ko`

---

## 6) Permissions (important): iTerm2 Automation

OMO Talk controls iTerm2 using Apple Events.

When macOS asks for permission (first run / first session creation), you must allow it.

If permissions get stuck, check:

- System Settings → Privacy & Security → Automation
- Enable OMO Talk → iTerm2

Symptoms:

- iTerm2 does not open when you click Add Session
- attach does not work

---

## 7) Troubleshooting

### 7-1) Telegram messages are ignored

Possible causes:

- **Bot is stopped**: check Start/Stop in the menubar app
- **Allowed User ID mismatch**: Allowed User ID must match your Telegram `from.id`; only that ID is accepted
- **Group chat + someone else sent the message**: in groups, `from.id` belongs to the sender; if Allowed User ID is your ID, messages from other participants are ignored
- **Token/settings not saved**: Settings may not have been saved, or you're running a different instance/mac with different settings

Recommended fix order:

1) Temporarily clear Allowed User ID in Settings and save
2) Send `/start` to the bot
3) Enter the returned user_id into Allowed User ID and save
4) In group chats, test by sending a message from your own account

### 7-2) opencode answers do not come back to Telegram

- Verify opencode is actually running in that session
- Check the Sessions list in the menubar app

### 7-3) Multi-line input (terminal)

This applies when you are **not in Telegram**, but directly interacting with opencode in iTerm2/tmux.

In tmux, `Shift+Enter` may not insert a new line.

- New line: `Ctrl+J`
- Send: `Enter`

---

## 8) Uninstall

1) Quit OMO Talk from the menubar
2) Delete `/Applications/OMO Talk.app`

Session mapping data is stored in your user library. (Optional manual cleanup)

- `~/Library/Application Support/omotalk/`

---

## 9) Additional notes

- OMO Talk works without a server; it relays Telegram messages to tmux/iTerm2 on your Mac.
- It does not collect or store your personal data or messages. (Only minimal local settings / session mapping)

---

## 한국어

이 문서는 **OMO Talk 하나만** 보고 설치/설정/사용까지 할 수 있도록, 최대한 자세히 적어둔 가이드입니다.

OMO Talk은 **텔레그램 메시지 → tmux 세션(터미널) → opencode** 흐름으로 작업을 이어갈 수 있게 해주는 macOS 메뉴바 앱입니다.

---

## 0) 준비물 체크 (간단)

아래 4가지만 먼저 확인하세요.

1) macOS 13 이상
2) iTerm2 설치
3) tmux 설치 (`tmux -V` 동작)
4) opencode + oh-my-opencode 설치 (`opencode --version` 동작)

---

## 1) 설치(DMG)

1) `OMO Talk.dmg`를 더블클릭합니다.
2) `OMO Talk.app`을 `Applications`로 드래그합니다.
3) `/Applications/OMO Talk.app`을 실행합니다.

버전 확인:

- Finder -> `/Applications` -> `OMO Talk.app` 선택 -> `Command + I` -> 버전 확인

---

## 2) 빠른 시작 (앱 내부와 동일)

1) opencode와 oh-my-opencode를 설치합니다.
2) tmux를 설치합니다.
3) 텔레그램에서 `@BotFather`를 찾습니다.
4) `/newbot`으로 새 봇을 만들고 토큰을 복사합니다.
5) OMO Talk 설정에서 **Bot Token**을 붙여넣습니다.
6) **저장(Save)** 버튼을 누릅니다.
7) 메인 패널에서 **봇 시작(Start Bot)** 버튼을 누릅니다.
8) 텔레그램에서 봇에게 아무 메시지나 보냅니다.
9) 답장으로 받은 `user_id`를 **Allowed User ID**에 입력하고 다시 저장합니다.
10) **세션 추가(Add Session)**를 누르거나 텔레그램에서 `/new <별명> <경로>`를 입력합니다.

예시:

- 프로젝트가 `/Users/user/Desktop/ppap`이고 Base Path가 `/Users/user/Desktop`이면 `/new ppap /ppap` 사용 가능

---

## 3) 경로 입력 규칙 (`/new` / 세션 추가)

- 전체 절대경로: `/Users/you/...`
- 홈 경로: `~/...`
- 상대경로: Base Path 기준 (예: `project`)
- 편의 보정: Base Path가 `/Users/you/Desktop`일 때 `/new app /app` 입력 시 `/Users/you/Desktop/app`로 해석 가능

---

## 4) 텔레그램에서 사용하기

### 4-1) 기본 사용(별명/경로 지정)

세션에 프롬프트를 보내는 기본 형식:

```text
<alias|path> ~~ <prompt>
```

예:

```text
app ~~ 지금 프로젝트 구조를 요약해줘
```

### 4-2) 채팅방별 기본 세션 설정(/base)

텔레그램은 **봇과의 개인 채팅**뿐 아니라, **그룹 채팅방**에서도 봇을 초대해 사용할 수 있습니다.
이걸 이용하면 예를 들어:

- `프로젝트 A` 그룹방 → tmux 세션 A
- `프로젝트 B` 그룹방 → tmux 세션 B

처럼 **채팅방 1개 = 세션 1개**로 매핑해두고,
각 방에서 `/base`를 한 번만 설정하면 이후에는 `alias ~~`를 매번 붙이지 않아도 됩니다.

방(A, B, C 등)을 여러 개 쓰는 경우, 매번 `alias ~~`를 붙이기 귀찮다면 채팅방별 기본 세션을 지정하세요.

1) 해당 채팅방에서:

```text
/base <alias|path>
```

예:

```text
/base app
```

2) 이제 그 채팅방에서는 **그냥 메시지만 보내도** 기본 세션으로 라우팅됩니다.

```text
오늘 할 일 정리해줘
```

3) 기본 세션 해제:

```text
/base clear
```

### 4-3) /base가 설정돼 있어도 다른 세션으로 보내기

채팅방 기본 세션이 `A`로 되어 있어도, 아래처럼 보내면 그 메시지만 `B` 세션으로 들어갑니다.

```text
B ~~ 이건 B 세션으로 보내줘
```

---

## 5) 텔레그램 명령어 전체

아래는 `/help`에서 확인할 수 있는 명령어들입니다.

- `/new <path>`
- `/new <alias> <path>`
- `/attach <alias|path>`
- `/list`
- `/status <alias|path>`
- `<alias|path> ~~ <prompt>`
- `/close <alias|path>`
- `/base <alias|path|clear>`
- `/lang en|ko`

---

## 6) 권한(중요): iTerm2 Automation 허용

OMO Talk은 iTerm2를 Apple Events로 제어합니다.

처음 실행/처음 세션 생성 시 macOS가 권한을 물어보면 **허용**해야 합니다.

권한이 꼬였을 때 확인 경로:

- System Settings → Privacy & Security → Automation
- OMO Talk이 iTerm2를 제어하도록 체크

증상 예:

- 세션 추가를 눌러도 iTerm2가 열리지 않음
- attach가 동작하지 않음

---

## 7) 문제 해결(자주 발생)

### 7-1) 텔레그램에서 메시지를 보냈는데 무시됨

가능한 원인들:

- **봇이 꺼져 있음**: 메뉴바에서 Start/Stop 상태를 확인
- **Allowed User ID 불일치**: Allowed User ID는 "내 계정"의 `from.id`이고, 그 ID에서 보낸 메시지만 처리됨
- **그룹방에서 다른 사람이 보낸 메시지**: 그룹방이라도 `from.id`는 "메시지 보낸 사람" 기준이라, Allowed User ID가 내 ID면 다른 참가자의 메시지는 무시됨
- **토큰/설정 저장 누락**: Settings에서 저장이 안 됐거나(또는 다른 Mac에서 다른 설정으로 실행 중) 토큰이 다른 봇 토큰일 수 있음

해결 순서(권장):

1) Settings에서 Allowed User ID를 잠깐 비우고 저장
2) 텔레그램에서 봇에게 `/start` 전송
3) OMO Talk이 답장해주는 user_id를 Allowed User ID에 다시 입력 후 저장
4) 그룹방을 쓰는 경우, "내가 직접" 메시지를 보내서 동작 확인

### 7-2) opencode 답변이 텔레그램으로 안 옴

- 해당 세션에서 opencode가 실제로 실행 중인지 확인
- 메뉴바 앱에서 Sessions 목록이 보이는지 확인

### 7-3) 멀티라인 입력(터미널에서)

이 항목은 **텔레그램이 아니라, PC에서 iTerm2/tmux 안의 opencode 화면을 직접 조작할 때**에 해당합니다.

tmux 안에서는 `Shift+Enter`가 줄바꿈으로 안 먹을 수 있습니다.

- 줄바꿈: `Ctrl+J`
- 전송: `Enter`

---

## 8) 제거(언인스톨)

1) 메뉴바에서 OMO Talk 종료
2) `/Applications/OMO Talk.app` 삭제

세션 매핑 데이터는 사용자 라이브러리에 저장됩니다. (원하면 수동 삭제)

- `~/Library/Application Support/omotalk/`

---

## 9) 추가 안내

- OMO Talk은 서버 없이 동작하며, 텔레그램 메시지를 사용자의 Mac에서 tmux/iTerm2로 전달하는 방식입니다.
- 개인정보/메시지를 별도로 수집하거나 저장하지 않습니다. (설정/세션 매핑 정도만 로컬에 저장)
