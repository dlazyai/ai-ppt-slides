# 설치 및 설정

## 한 문장으로 설치하기

아래 문장을 Codex에 직접 보내 설치를 맡기는 것을 권장합니다.

```text
이 dlazy-ppt skill을 설치해 주세요. 링크: https://github.com/dlazyai/ai-ppt-slides
```

## Codex 수동 설치

명령줄에서 다음 명령을 실행해 `dlazy-ppt` skill을 Codex의 전역 skills 디렉터리에 설치합니다.

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent codex \
  --global
```

설치 후 Codex를 재시작하면 새 skill이 적용됩니다.

[GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases)에서 `ai-ppt-slides-v*.zip`을 다운로드해 압축을 푼 뒤, 그 안의 `dlazy-ppt` 폴더를 `~/.codex/skills/dlazy-ppt`에 넣고 Codex를 재시작하는 방법도 있습니다.

이 저장소를 로컬에서 개발하는 경우, 실시간으로 수정 사항을 테스트할 수 있도록 skill 디렉터리를 Codex skills 디렉터리에 심볼릭 링크로 연결할 수 있습니다.

```bash
mkdir -p ~/.codex/skills
ln -s /path/to/ai-ppt-slides/skills/dlazy-ppt ~/.codex/skills/dlazy-ppt
```

## OpenClaw 설치

```bash
openclaw skills install dlazy-ppt
```

OpenClaw의 skill allowlist를 사용하는 경우 허용 목록에 `dlazy-ppt`를 추가해야 합니다.

## Claude Code / Hermes Agent

Claude Code:

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent claude-code \
  --global
```

Hermes Agent:

```bash
npx -y skills@latest add dlazyai/ai-ppt-slides \
  --skill dlazy-ppt \
  --agent hermes-agent \
  --global
```

일반적인 대상 디렉터리는 Claude Code의 경우 `~/.claude/skills/dlazy-ppt`, Hermes Agent의 경우 `~/.hermes/skills/dlazy-ppt`입니다. 로컬 개발 시에는 복사 대신 심볼릭 링크를 사용할 수도 있습니다.

## skill 업데이트

아래 문장을 사용 중인 agent에게 직접 보내 업데이트를 맡기는 것을 권장합니다.

```text
dlazy-ppt skill을 최신 버전으로 업데이트해 주세요. 저장소: https://github.com/dlazyai/ai-ppt-slides
```

수동으로 업데이트할 때는 위에서 해당 agent에 맞는 설치 명령을 다시 실행하면 설치된 skill이 최신 버전으로 덮어써집니다. 또는 [GitHub Releases](https://github.com/dlazyai/ai-ppt-slides/releases)에서 최신 `ai-ppt-slides-v*.zip`을 다운로드해 압축을 풀고 기존 `dlazy-ppt` 디렉터리를 교체할 수 있습니다. 업데이트 후 agent를 재시작하면 적용됩니다.

업데이트는 안전합니다. API key 등의 런타임 설정은 `~/.dlazy-ppt/.env`에, 개인 스타일 라이브러리는 `~/.dlazy-ppt/references/`에 저장되며 모두 skill 설치 디렉터리 외부에 있습니다. 따라서 skill을 업데이트하거나 다시 설치해도 사라지지 않습니다. 각 버전의 변경 사항은 [Releases 페이지](https://github.com/dlazyai/ai-ppt-slides/releases) 또는 저장소의 `CHANGELOG.md`에서 확인할 수 있습니다.

## dLazy API key 설정

이미지 생성에는 dLazy API key가 필요합니다. 한 번만 설정하면 모든 agent가 함께 사용합니다.

1. [dlazy.com](https://dlazy.com)에 로그인한 뒤 [API key 페이지](https://dlazy.com/dashboard/organization/api-key)에서 key를 복사합니다.
2. 다음을 실행합니다:

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config --api-key "your-dlazy-api-key"
```

key를 agent에게 건네고 대신 저장해 달라고 요청해도 됩니다.

설정은 `~/.dlazy-ppt/.env`에 권한 `0600`으로 저장됩니다. Codex, Claude Code, OpenClaw, Hermes Agent가 모두 이 파일 하나를 읽으며, skill을 업데이트하거나 재설치해도 사라지지 않습니다.

## 설정 확인

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py doctor --check-api
```

이 명령은 공유 런타임을 점검하고 현재 설정을 출력한 뒤, 계정의 도구 목록을 가져와 key가 유효하고 모델을 사용할 수 있는지 확인합니다.

- `HTTP 401`: key가 유효하지 않거나 폐기되었습니다. API key 페이지에서 새로 발급하세요.
- `insufficient_balance`: 조직의 크레딧이 부족합니다. [크레딧 페이지](https://dlazy.com/dashboard/organization/settings?tab=credits)에서 충전하세요.
- 모델이 목록에 없다는 안내: 해당 계정에 그 이미지 도구 권한이 없습니다.

## 선택 설정

- `DLAZY_BASE_URL`: 자체 호스팅 배포에만 필요하며 기본값은 `https://dlazy.com`입니다.
- `DLAZY_PPT_IMAGE_MODEL`: 다른 dLazy 이미지 도구를 사용할 때 지정하며 기본값은 `gpt-image-2`입니다.

둘 다 같은 `config` 명령으로 기록합니다:

```bash
python3 {skill_root}/scripts/dlazy_ppt_runtime.py config \
  --api-key "your-dlazy-api-key" \
  --base-url "https://dlazy.example.com" \
  --model gpt-image-2
```
