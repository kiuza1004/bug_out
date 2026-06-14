# 🦟 BugOut

브라우저에서 바로 동작하는 **초음파 해충 퇴치 웹앱**. 설치/빌드 없이 정적 HTML 한 장으로 동작.

**👉 Live Demo:** https://kiuza1004.github.io/bug_out/

## 특징

- 🎚 **주파수 슬라이더** (100 Hz ~ 22 kHz)
- 🐛 **해충별 프리셋**: 모기 / 파리 / 날파리 / 쥐 / 바퀴벌레 / 거미
- 🎵 **파형 선택**: Sine / Square / Sawtooth / Triangle
- 🔁 **주파수 스윕**: Off / Slow / Fast
- 🔉 **클릭 노이즈 없는 ramp-in/out**
- 📱 모바일 친화 다크 UI, 탭 숨김 시 자동 정지
- 🪶 의존성 0, 빌드 0 — `index.html` 한 파일

## 로컬 실행

```bash
# 아무 정적 서버나 OK
npx serve .
# 또는
python -m http.server 8080
```

브라우저에서 `http://localhost:8080` 접속.

## 배포 (GitHub Pages)

`main` 브랜치 루트의 `index.html`이 Pages에서 자동 서빙됨.
설정: **Settings → Pages → Source: `main` / root**.

## 주의

초음파의 해충 퇴치 효과는 종·환경·기기 스피커 성능에 따라 다르며 과학적 근거가 제한적입니다.
반려동물(특히 개·고양이·햄스터·새)이 불편해할 수 있고, 청력이 민감한 사람은 두통이 발생할 수 있습니다.
**볼륨을 낮게 시작하고, 이어폰/헤드폰 착용 시 사용하지 마세요.**

## 기술 스택

- Vanilla JS + Web Audio API (`OscillatorNode`, `GainNode`)
- Pure CSS (CSS variables, grid, gradient)
- 빌드 도구 없음

## 라이선스

MIT
