# Oh My Flunky — 로컬 데몬

Oh My Flunky 를 사용하려면 각자 PC 에서 **로컬 데몬**이 실행되고 있어야 합니다.
데몬은 에이전트 · 스킬 · 워크플로우 · 지식 · 도구 · 실행 이력과 **AI 모델 키**를
전부 **내 PC 안에만** 저장하고, AI 실행도 이 프로세스 안에서 처리합니다.
어떤 도메인 데이터도, API 키도 네트워크로 밖으로 나가지 않습니다.

> 중앙 서버는 **Google 로그인(신원 확인)** 만 담당합니다. 나머지는 전부 로컬 데몬이 처리합니다.

---

## 1. 다운로드

최신 릴리스에서 내 OS 에 맞는 파일을 받으세요.

| OS | 파일 |
|----|------|
| **macOS** | [`oh-my-flunky-daemon-macos`](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest/download/oh-my-flunky-daemon-macos) |
| **Windows** | [`oh-my-flunky-daemon.exe`](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest/download/oh-my-flunky-daemon.exe) |

또는 [릴리스 페이지](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest)에서 직접 받을 수 있습니다.

---

## 2. 설치 & 실행

**처음 한 번만 실행하면 됩니다.** 데몬이 스스로 자동시작을 등록하고 백그라운드로 돌아갑니다.
이후에는 **재부팅하거나 로그인할 때마다 자동으로 켜집니다.**

### macOS

```bash
# 1) 받은 파일에 실행 권한 부여
chmod +x ~/Downloads/oh-my-flunky-daemon-macos

# 2) (보안 경고 방지) 격리 속성 제거 — "확인되지 않은 개발자" 경고가 뜰 때
xattr -d com.apple.quarantine ~/Downloads/oh-my-flunky-daemon-macos 2>/dev/null

# 3) 실행 — 최초 실행 시 자동시작(LaunchAgent) 등록 + 백그라운드 구동
~/Downloads/oh-my-flunky-daemon-macos
```

- 실행하면 즉시 백그라운드로 돌아가고, 이후 **로그인할 때마다 자동 시작**됩니다.
- 죽어도 자동으로 다시 살아납니다(`KeepAlive`).

> Finder 에서 더블클릭해도 됩니다. "확인되지 않은 개발자" 경고가 뜨면
> **우클릭 → 열기** 로 한 번만 허용해 주세요.

### Windows

1. `oh-my-flunky-daemon.exe` 를 **더블클릭**합니다.
2. 최초 실행 시 자동시작(작업 스케줄러)이 등록되고 백그라운드로 구동됩니다.
   - 별도의 창은 뜨지 않습니다(백그라운드 실행).
   - 이후 **로그인할 때마다 자동 시작**됩니다.

> SmartScreen 경고가 뜨면 **추가 정보 → 실행** 을 눌러 주세요.

---

## 3. 동작 확인

데몬이 켜져 있으면 Oh My Flunky 화면 좌측 하단에 **"데몬 연결됨"** 초록불이 표시됩니다.
새로고침해도 자동으로 다시 연결되며, 데몬을 뒤늦게 켜도 몇 초 안에 자동으로 붙습니다.

데몬은 로컬 주소 `http://127.0.0.1:18799` 에서 동작합니다.

---

## 4. 명령어 옵션

| 명령 | 설명 |
|------|------|
| _(옵션 없음)_ | **최초 실행**: 자동시작 서비스 등록 + 백그라운드 구동 (권장) |
| `--install` | 자동시작 서비스만 수동 등록 |
| `--uninstall` | 자동시작 서비스 제거 |
| `--foreground` | 서비스 없이 **포그라운드**로 실행 (로그를 터미널에서 바로 보고 싶을 때) |
| `--no-service` | 서비스 등록 없이 **이 세션에서만** 백그라운드 구동 |

예시:
```bash
# 자동시작 해제(제거)
./oh-my-flunky-daemon-macos --uninstall     # macOS
oh-my-flunky-daemon.exe --uninstall         # Windows
```

---

## 5. 로그 · 문제 해결

- **로그 위치 (macOS)**: `/tmp/oh-my-flunky-daemon.log`
- **"데몬 미실행" 이 계속 뜰 때**
  - 데몬을 실행했는지 확인하고, 앱을 새로고침(`Cmd/Ctrl+Shift+R`)하세요.
  - 그래도 안 되면 데몬을 다시 실행하세요. 포트(`18799`)가 다른 프로그램에 점유돼 있지 않은지 확인하세요.
- **macOS 보안 경고**: 위 2번의 `xattr` 명령 또는 **우클릭 → 열기** 로 허용하세요.
- **완전 제거**: `--uninstall` 로 자동시작을 해제한 뒤 내려받은 실행 파일을 삭제하면 됩니다.

---

## 자동 업데이트

앱은 항상 **최신 릴리스(latest)** 의 데몬을 받도록 안내합니다.
새 버전이 배포되면 위 다운로드 링크에서 최신 파일을 다시 받아 한 번 실행하면 갱신됩니다.
