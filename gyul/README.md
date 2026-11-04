# Project: 귤 까기 장인 (Mandarin Peeling Master)

[한국어 설명은 여기를 참고하세요.](#한국어-요약)

A 3D casual browser game about peeling a mandarin into art.

A pattern is drawn on the mandarin in marker. Press, trace and roll the fruit to score the peel along the lines before time runs out.
When the round ends, your peel is laid flat so you can see the shape you actually made.

**▶ Play now:** https://lalunru.github.io/mini-games/gyul/ (no install, works on mobile and desktop)

## Gameplay

| Action | Control |
| --- | --- |
| Score the peel | 🔪 Peel mode: press on the mandarin and trace the dashed marker line |
| Roll the mandarin | 🖐 Roll mode, drag outside the fruit, two-finger drag, right-click drag, or arrow keys |
| Keep peeling around the side | While peeling, drag past the edge and the mandarin rolls toward you as the cut continues |
| Switch mode | `Space` |

### Patterns

| Pattern | Difficulty | Time | What makes it hard |
| --- | --- | --- | --- |
| Flower (꽃) | ★ | 25s | 6 lines from the stem down |
| Heart (하트) | ★ | 30s | One closed loop around the fruit |
| Octopus (문어) | ★★ | 40s | 8 longer lines, more of them on the back |
| Star (별) | ★★ | 40s | Sharp corners at five points |
| Swirl (회오리) | ★★★ | 45s | One continuous line, 2.5 turns. You have to roll while peeling |
| Person (사람) | ★★★ | 45s | Head, arms and legs, all the way around |

Closed shapes (heart, star, person) work like real mandarin art: once the outline is fully cut, the outer peel falls away and only the shape stays attached.
Press **다 깠어요** (Done) to finish early. Time left becomes a bonus once progress is 80% or more.

### Scoring

- **Progress**: how much of the pattern line you have traced
- **Accuracy**: how much of your cutting stayed near the line
- **Score** = 1000 × progress × (0.6 + 0.4 × accuracy), plus a time bonus if you finish early

| Score | Rank |
| --- | --- |
| 900+ | 귤 까기 장인 (Mandarin Peeling Master) |
| 700+ | 손이 야무진 사람 (Skilled hands) |
| 450+ | 귤은 맛있으면 돼 (It still tastes good) |
| below 450 | 그냥 손으로 먹자 (Just eat it) |

## Design

**One gesture that feels like the real thing.** Peeling is press and trace. Rolling is the other half of the real action, so it got its own mode and gestures.

**The result is the reward.** At the end, the peel unfolds from a closed bud into a flat shape, next to the target pattern.
The unfold is computed from your actual cuts, so every result is different and worth sharing.

**Data-driven tuning.** Cut width, line tolerance, rolling speed and time bonus all live in a single `CONFIG` object at the top of the code.

## Balance Tuning Log

**v1 → v2: "The mandarin doesn't roll well"**

Playtest feedback said the mandarin was hard to roll.

Causes found:
1. The fruit filled most of the screen, so there was almost no empty space to drag for rolling.
2. Edge auto-roll started too late and was too slow.
3. Peeling stopped completely when the finger slid off the fruit.

| Change | v1 | v2 |
| --- | --- | --- |
| Rolling input | Drag outside the fruit only | + Roll mode button, two-finger drag, right-click drag, arrow keys |
| Momentum | None | Fruit keeps rolling briefly after release |
| Edge auto-roll starts at | Facing 0.55 | Facing 0.72 (earlier) |
| Edge auto-roll speed | 2.4 | 4.0 |
| Dragging past the edge while peeling | Cut stops | Fruit rolls toward the finger and the cut continues |
| Fruit size on screen | ~62% | ~50–60% (leaves room to drag) |

The biggest change was a new interaction rather than a number:
**drag past the edge while peeling, and the mandarin rolls under the knife.**
It turned the hardest pattern (Swirl) from frustrating into the most satisfying one.

**v2 → v3: "Rounds are too long", "I want a finish button", "More shapes, like a person"**

| Feedback | Change |
| --- | --- |
| Rounds feel too long | Time cut by about 40% (Flower 45s → 25s). Each limit is now set from the pattern's line length, about 25° of peel per second |
| No way to end early | Added **다 깠어요** (Done). Time bonus = time left × progress, only at 80%+ progress, so quitting early is never better than finishing |
| Shapes feel samey | Added Heart, Star and Person. These are closed outlines designed directly on the unfolded peel, then mapped back onto the sphere |

Closed shapes needed a new rule: **peel fully surrounded by cuts falls off.**
At the end, a flood fill keeps only the peel still connected to the bottom of the mandarin, so a finished outline leaves exactly the shape, and an unfinished one leaves the outer peel attached.

## Tech Stack

| | |
| --- | --- |
| Rendering | three.js (r160), WebGL |
| Language | JavaScript (single `index.html`, no build step) |
| Peel simulation | Cut mask on an equirectangular map, sphere-to-plane unfolding (azimuthal projection) |
| Audio | Web Audio API (procedurally generated tearing sound) |
| Platform | Browser, mobile and desktop |
| Deploy | GitHub Pages |
| Development | Solo project, built with AI-assisted (vibe) coding |

## License

Personal project. All rights reserved.

---

## 한국어 요약

귤 위에 마커로 그려진 모양 틀을 따라 껍질을 까는 3D 캐주얼 웹게임입니다.
귤을 꾹 누른 채 선을 따라 긋고, 굴려가며 뒷면까지 까야 합니다. 시간이 끝나면 내가 깐 껍질이 평면으로 펼쳐지고, 목표 모양과 비교해 점수와 등급이 나옵니다.

**핵심 설계**

- 조작은 실제 귤 까기처럼 "누르고 긋기"와 "굴리기" 두 가지입니다.
- 결과 화면이 곧 보상입니다. 실제로 그은 칼집으로 펼친 모양을 계산하기 때문에 매판 결과가 다르고, 공유하고 싶어집니다.
- 칼집 굵기, 판정 범위, 굴리는 속도, 시간 보너스는 모두 코드 상단 `CONFIG`에서 조정합니다.

**밸런스 튜닝 기록 (v1 → v2)**

- 문제: 플레이 테스트에서 "귤이 잘 안 굴러간다"는 피드백이 나왔습니다.
- 원인: 귤이 화면을 너무 많이 차지해 바깥을 끌 공간이 없었고, 가장자리 자동 굴리기는 늦고 느렸으며, 손가락이 귤 밖으로 나가면 까기가 멈췄습니다.
- 해결: 굴리기 모드 버튼, 두 손가락·방향키 입력, 관성을 추가하고 자동 굴리기를 빠르게 했습니다. 가장 큰 변화는 **긋다가 귤 밖으로 끌고 나가면 귤이 그쪽으로 굴러오며 칼집이 이어지는 조작**을 새로 넣은 것입니다. 가장 어려운 회오리 모양이 이 조작 덕분에 답답함에서 손맛으로 바뀌었습니다.

**밸런스 튜닝 기록 (v2 → v3)**

- 피드백: "시간이 너무 길다", "끝내기 버튼이 있으면 좋겠다", "사람 모양처럼 더 다채로우면 좋겠다"
- 시간: 약 40% 줄였습니다(꽃 45초 → 25초). 모양마다 선 길이를 재서 초당 약 25도를 까는 속도로 제한 시간을 맞췄습니다.
- 끝내기: **다 깠어요** 버튼을 추가했습니다. 시간 보너스는 "남은 시간 × 진행도"이고 진행도 80% 이상일 때만 주기 때문에, 대충 까고 빨리 끝내는 쪽이 이득이 되지 않습니다.
- 모양: 하트, 별, 사람을 추가해 6종이 됐습니다. 펼친 껍질 평면에서 윤곽을 먼저 그리고 그걸 구면으로 옮겨 모양 틀을 만들었습니다. 그리고 **칼집으로 완전히 둘러싸인 껍질은 떨어져 나가는 규칙**을 넣어, 테두리를 다 까면 바깥 껍질이 떨어지고 모양만 남게 했습니다.

**기술 스택**: three.js, JavaScript(단일 HTML), Web Audio API, GitHub Pages / 1인 개발(AI 바이브코딩 활용)
