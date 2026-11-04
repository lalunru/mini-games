# Project: 막내의 회식 (The Rookie's Company Dinner)

[한국어 설명은 여기를 참고하세요.](#한국어-요약)

A 3D casual browser game about the most stressful job at a Korean company dinner (회식):
grilling samgyeopsal for your boss.

You are the newest hire. Grill the pork belly, cut it into bite-size pieces,
and keep your boss's plate full. Read the room. Survive 2 minutes.

**▶ Play now:** https://lalunru.github.io/mini-games/hoesik/ (no install, works on mobile and desktop)

## Gameplay

The core loop has four steps:

**Place → Grill & flip → Cut → Serve**

| Action | Control |
| --- | --- |
| Put raw meat on the grill | 🥩 button or tap the raw meat tray (`E` / `Space`) |
| Flip | Tongs mode, tap a piece (`Q`) |
| Move / Serve | Tongs mode, drag a piece onto the boss's plate |
| Cut | Scissors mode, swipe across a strip (`W`). One long swipe can cut several strips at once |
| Throw away | Drag a burnt piece into the leftover bowl |

### Boss mood system

Your boss's mood (0–100) is the only score. It decides your final rank.

| Event | Mood |
| --- | --- |
| Perfectly cooked, bite-size piece | +10 |
| Crispy piece | +4 |
| Undercooked / raw / burnt | −8 / −18 / −14 |
| Too big / too small / served as a whole strip | −6 / −4 / −10 |
| Plate left empty | Drains over time |
| Smoke from burnt meat on the grill | Drains over time |

Each side of a piece cooks on its own. Heat is strongest at the center of the grill.
You only see the top side, so you have to judge the bottom by timing and by the color on the edges, just like the real thing.

### Ranks

| Final mood | Rank |
| --- | --- |
| 90+ | 고속 승진 예정 (Fast-track promotion) |
| 72+ | 센스 있는 막내 (The rookie who gets it) |
| 52+ | 무난한 신입 (Average new hire) |
| 30+ | 눈치 교육 대상 (Needs social-awareness training) |
| below 30 | 회식 열외 확정 (Excused from future dinners) |

The result card can be copied and shared.

## Design

**One input, one judgment.** Every action uses one gesture: tap, drag, or swipe.
The depth comes from timing and judgment, not from how many controls you have to learn.

**Data-driven balance.** Every tuning value lives in a single `CONFIG` object at the top of the code:
cooking speed, doneness thresholds, bite size, boss patience, and score weights.
The boss's lines (`LINES`) and ranks (`RANKS`) are data too, so the tone can be changed without touching the logic.

**Piece state machine.** Each meat piece moves through `drop → grill ⇄ flip → drag → plate / trash / return → eaten`.
Cutting replaces one piece with two new ones that keep both sides' doneness.

## Balance Tuning Log

**v1 → v2: "The boss gets angry before the meat is even cooked"**

Playtest feedback said the boss was upset before the first piece could be served.
Calculating the fastest possible first serve (grill, flip, grill, cut) gave about 17 seconds.
The boss's total patience at the start was also exactly 17 seconds (12s grace + 5s patience),
so even perfect play started losing mood.

| Value | v1 | v2 |
| --- | --- | --- |
| Starting mood | 65 | 70 |
| Start grace period | 12s | 22s |
| Empty-plate patience | 5s | 8s |
| Empty-plate drain | 2.5/s | 1.5/s |
| Boss eating interval | 3.2s | 4.5s |
| Cooking speed | 0.09 | 0.11 |

A new rule was added instead of only raising numbers: **if meat is grilling, the boss waits.**
While meat is on the grill, the empty-plate drain is reduced to 40%,
and the boss's line changes from "고기가... 없네?" (No meat...?) to "고기 언제 익나~?" (When's it gonna be ready~?).
This matches how a real company dinner feels.

## Tech Stack

| | |
| --- | --- |
| Rendering | three.js (r160), WebGL |
| Language | JavaScript (single `index.html`, no build step) |
| Audio | Web Audio API (procedurally generated sizzle and cut sounds, no audio files) |
| Platform | Browser, mobile and desktop (responsive camera and layout for portrait screens) |
| Deploy | GitHub Pages |
| Development | Solo project, built with AI-assisted (vibe) coding |

## License

Personal project. All rights reserved.

---

## 한국어 요약

회식 자리에서 사회초년생이 맡는 가장 긴장되는 업무, **부장님 고기 구워 드리기**를 옮긴 3D 캐주얼 웹게임입니다.
삼겹살을 굽고, 뒤집고, 한입 크기로 잘라 부장님 앞접시가 비지 않게 대접해야 합니다. 제한 시간은 2분이고, 끝날 때 부장님 기분에 따라 "고속 승진 예정"부터 "회식 열외 확정"까지 등급이 나옵니다.

**핵심 설계**

- 조작은 탭, 드래그, 긋기 세 가지뿐입니다. 재미는 조작 수가 아니라 타이밍과 판단에서 나오도록 했습니다.
- 고기는 윗면과 아랫면이 따로 익고, 불판 가운데일수록 빨리 익습니다. 보이는 건 윗면뿐이라 아랫면은 시간과 옆면 색으로 판단해야 합니다.
- 밸런스 수치, 부장님 대사, 등급은 모두 코드 상단 데이터(`CONFIG`, `LINES`, `RANKS`)로 분리해 로직 수정 없이 튜닝할 수 있습니다.

**밸런스 튜닝 기록 (v1 → v2)**

- 문제: 플레이 테스트에서 "고기가 익기도 전에 부장님 기분이 나빠진다"는 피드백이 나왔습니다.
- 원인: 첫 조각을 드릴 수 있는 최단 시간이 약 17초인데, 시작 시 부장님 인내 시간도 17초(유예 12초 + 인내 5초)여서 완벽하게 플레이해도 기분이 깎였습니다.
- 해결: 유예와 인내 시간, 먹는 간격, 익는 속도를 조정했습니다. 여기에 더해 **불판에 고기가 굽고 있으면 부장님이 기다려 주는 규칙**(기분 감소 40%로 완화, 대사 변경)을 새로 넣어, 수치 조정만이 아니라 실제 회식 상황에 맞는 방식으로 풀었습니다.

**기술 스택**: three.js, JavaScript(단일 HTML), Web Audio API, GitHub Pages / 1인 개발(AI 바이브코딩 활용)
