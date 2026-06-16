# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

BugOut — 정적 HTML 한 장으로 동작하는 초음파 해충 퇴치 웹앱. 빌드 시스템·의존성·번들러 없음. `index.html` 단일 파일이 곧 앱이자 배포 산출물.

배포: GitHub Pages (`main` 브랜치 root). 푸시 = 배포.

## Commands

```bash
# 로컬 미리보기 (둘 중 아무거나)
npx serve .
python -m http.server 8080

# 배포
git push origin main         # → 1~2분 내 https://kiuza1004.github.io/bug_out/ 반영
```

테스트 러너·린터·빌드 스텝 없음. 변경 검증은 브라우저에서 실제 재생/슬라이더/프리셋을 눌러보는 것으로 한다 (특히 초음파 영역 17~22kHz는 사람 귀로 확인 어려움 — 스펙트럼 분석기/주파수 측정 앱 또는 가청 대역 1kHz로 임시 변경 후 동작 확인).

## Architecture

전부 `index.html` 안에 인라인. 분리 파일 만들지 말 것 — "1 파일 = 1 앱 = 1 배포" 가 이 프로젝트의 의도된 제약이다.

핵심 구조 (`index.html` 내부):

- **`<style>`** — CSS 변수 기반 다크 테마. 색은 `:root`의 `--bg/--fg/--accent/...` 만 수정.
- **`PRESETS` 배열** — 해충별 `{id, name, hz, wave, sweep, emoji}`. 프리셋 추가는 이 배열에 push 하면 UI 자동 렌더 (`buildPresets()`).
- **`state` 객체** — 단일 진실 공급원: `freq, volume, wave, sweep, playing, activePreset`. UI는 항상 `render()`가 state → DOM 으로 단방향 반영.
- **Web Audio 그래프** — `OscillatorNode → GainNode → destination`. 모듈 변수 `ctx/osc/gain` 한 쌍만 유지. 재생 시작/중지마다 oscillator를 새로 만들고 (`startTone`/`stopTone`), 파라미터 변경(주파수·볼륨)은 기존 노드에 `setTargetAtTime`으로 부드럽게 적용.
- **스윕** — `setInterval` + `Math.sin` 위상으로 base 주파수 ±1.5kHz 진동. `osc.frequency.setTargetAtTime`으로 글라이드.
- **클릭 노이즈 방지** — start/stop 시 50ms `linearRampToValueAtTime` ramp. 이 패턴은 깨지 말 것 — 제거하면 톤 시작·종료 시 "툭" 소리 발생.
- **visibilitychange 핸들러** — 탭 숨김 시 자동 정지. 의도된 동작 (배경 재생 방지).

## Conventions

- **새 파형/스윕 모드 추가**: HTML의 `<div class="toggle">` 안에 `<button class="chip" data-...>` 추가 + state 분기 처리. 토글은 click delegation으로 처리되므로 핸들러 수정 불필요.
- **AudioContext는 lazy**: `ensureCtx()`가 첫 사용자 제스처 시점에 생성. 자동재생 정책 우회 위해 페이지 로드 시 만들지 말 것.
- **주파수 상한 22000Hz**: 슬라이더 max 및 스윕 clamp 모두에 하드코딩. Web Audio 기본 샘플레이트(44.1/48kHz) 나이퀴스트 한계 고려한 값.
- **CRLF 경고 무시**: Windows 환경. git이 자동 처리.

## Out of Scope (의도적으로 안 한 것)

빌드 도구, 프레임워크(React/Vue), 모듈 시스템, npm 의존성, PWA manifest/SW, 백엔드, 테스트 러너 — 어떤 것도 추가하지 말 것. 추가 요청이 오면 "이 프로젝트는 단일 파일 원칙"임을 먼저 환기.
