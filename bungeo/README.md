# Project: 붕어빵 노점 (Bungeoppang Street Stall)

[한국어 설명은 여기를 참고하세요.](#한국어-요약)

A 3D cooking-management browser game about running a Korean fish-shaped pastry stall.

Mix the batter from flour, pour it into the fish molds, add the filling, cover it, flip it, and sell it to customers before they lose patience.
Leave it too long and it burns.

**▶ Play now:** https://lalunru.github.io/mini-games/bungeo/ (no install, works on mobile and desktop)

## Gameplay

**Mix batter → Pour → Add filling → Pour again → Flip → Take out → Sell**

| Step | Control |
| --- | --- |
| Mix batter | Add flour, milk and sugar, then stir in circles inside the bowl (3 turns) |
| Pour | 🫖 Pour tool, press and hold on a mold. Pour about half first |
| Add filling | Pick 🫘 Red bean, 🍮 Custard, 🍠 Sweet potato or 🍕 Pizza, then tap the mold |
| Cover | Pour again on top of the filling |
| Flip | 🔄 Flip tool, tap the mold. Both sides need to be golden |
| Take out | 🥢 Take-out tool, tap the mold. The pastry goes to the display tray |
| Sell | Tap a customer card. Matching pastries from the tray are handed over |

Keyboard: `1` Pour, `2` Flip, `3` Take out, `Q` `W` `E` `R` fillings.

### What makes a good bungeoppang

| Check | Rule |
| --- | --- |
| Doneness | Each side cooks separately. Golden on both sides earns a tip; raw or burnt sells for nothing |
| Amount | Too little batter is skinny, too much overflows the mold |
| Covered | Filling not covered by the second pour leaks out |
| Filling | No filling means a half-price plain pastry that no order matches |

Customers order 1 to 3 pastries and leave angry when their patience runs out.
The round lasts 90 seconds, and the rank comes from total sales.

| Sales | Rank |
| --- | --- |
| 13,000원+ | 붕어빵 장인 (Bungeoppang Master) |
| 8,000원+ | 단골 생기는 집 (Regulars keep coming back) |
| 4,000원+ | 동네 붕어빵집 (Neighborhood stall) |
| below | 오늘은 문 닫자 (Let's close early today) |

## Design

**Every step by hand.** The whole process is playable, starting from flour. Running out of batter in the middle of a rush is part of the pressure.

**Six molds, one pair of hands.** Each mold cooks on its own timer, so the challenge is juggling: pouring one while another is about to burn.

**Data-driven tuning.** Pour speed, fill thresholds, cooking speed, prices, customer patience and arrival rate all live in a single `CONFIG` object at the top of the code.

## Balance Tuning Log

**v1 → v2: "It burns way too fast"**

The first side starts cooking the moment batter hits the mold, while the player is still adding the filling and pouring the cover.
In v1, a side reached "perfect" at about 8 seconds and burned at about 17, which left almost no room to juggle six molds.
There was also no warning: the color changed gradually, and smoke appeared only after it had already burned.

| Value | v1 | v2 |
| --- | --- | --- |
| Cooking speed | 0.09 | 0.055 (perfect at ~14s per side) |
| Perfect window ends | 1.25 | 1.35 |
| Burns at | 1.5 (~17s) | 1.7 (~31s) |
| Rank thresholds | 16,000 / 10,000 / 5,000원 | 13,000 / 8,000 / 4,000원 (slower cooking means fewer sales) |

New signals instead of only slower numbers:
- **✨** next to a mold when its bottom side is golden and ready to flip
- **⚠️** shortly before it starts to burn, with light steam rising
- **🔥** and dark smoke once a side has burned

The goal was to keep the pressure of juggling six molds, but make burning the player's fault rather than a surprise.

**v2 → v3: "Customers leave too quickly"**

Slowing down the cooking in v2 created a new problem. One pastry now takes at least ~30 seconds (mix, pour, fill, cover, two sides), but customers only waited 34 seconds, and the first one arrived 2 seconds in, while the batter was still being mixed.
Fixing one number broke another, so patience was re-derived from the new cooking time.

| Value | v2 | v3 |
| --- | --- | --- |
| Patience | 34s for any order | 55s + 12s per pastry ordered (67–91s) |
| Serving part of an order | No effect | +10s patience per pastry handed over |
| First customer arrives | 2s | 8s (time to mix the first batter) |
| Gap between customers | 5–9s | 8–13s |
| Patience bar | Green → red | Green → orange → red, with 😤 when almost gone |

Serving part of an order now buys time, which rewards handing over whatever is ready instead of waiting to complete the whole order.

## Tech Stack

| | |
| --- | --- |
| Rendering | three.js (r160), WebGL |
| Language | JavaScript (single `index.html`, no build step) |
| Audio | Web Audio API (procedurally generated sizzle and bell sounds) |
| Platform | Browser, mobile and desktop |
| Deploy | GitHub Pages |
| Development | Solo project, built with AI-assisted (vibe) coding |

## License

Personal project. All rights reserved.

---

## 한국어 요약

밀가루 반죽부터 판매까지 붕어빵 노점을 운영하는 3D 요리 경영 웹게임입니다.
반죽을 만들고, 틀에 붓고, 팥·슈크림·고구마·피자 중 소를 넣고, 다시 덮고, 뒤집어 양면을 구운 뒤 꺼내서 손님에게 팝니다. 오래 두면 탑니다.

**핵심 설계**

- 밀가루부터 모든 과정을 직접 합니다. 손님이 몰릴 때 반죽이 떨어지는 것도 긴장 요소입니다.
- 틀 6개가 각자 익어 가기 때문에, 한쪽에 붓는 동안 다른 쪽이 타지 않게 챙기는 멀티태스킹이 핵심입니다.
- 붓는 속도, 적정 반죽량, 익는 속도, 가격, 손님 인내심과 등장 간격은 모두 코드 상단 `CONFIG`에서 조정합니다.

**밸런스 튜닝 기록 (v1 → v2)**

- 피드백: "너무 금방 탄다"
- 원인: 첫 면은 반죽을 붓는 순간부터 익는데, 소를 넣고 덮는 동안에도 시간이 흐릅니다. v1에서는 약 8초에 완벽, 약 17초에 타서 틀 6개를 돌리기에 여유가 거의 없었습니다. 색이 서서히 변할 뿐 경고가 없었고, 연기도 이미 탄 뒤에야 났습니다.
- 해결: 익는 속도를 0.09 → 0.055로 낮추고(한 면 완벽까지 약 14초), 타는 기준을 1.5 → 1.7로 올려 약 31초까지 버티게 했습니다. 굽는 속도가 느려진 만큼 등급 매출 기준도 낮췄습니다.
- 신호 추가: 뒤집을 때가 되면 ✨, 타기 직전엔 ⚠️와 옅은 김, 타면 🔥와 검은 연기가 뜹니다. 여러 틀을 동시에 챙기는 긴장감은 유지하되, 타는 게 "갑자기"가 아니라 "내가 놓친 것"이 되도록 했습니다.

**밸런스 튜닝 기록 (v2 → v3)**

- 피드백: "손님이 너무 금방 간다"
- 원인: v2에서 굽는 속도를 늦추면서 붕어빵 하나에 최소 약 30초가 걸리게 됐는데, 손님 인내심은 34초 그대로였습니다. 첫 손님도 반죽을 젓고 있는 시작 2초 만에 왔습니다. 한 수치를 고치니 연결된 다른 수치가 깨진 경우라, 새 굽는 시간을 기준으로 인내심을 다시 계산했습니다.
- 해결: 인내심을 "55초 + 주문 개수당 12초"로 바꾸고, 주문 중 일부만 건네도 1개당 10초를 회복하게 했습니다. 첫 손님은 반죽을 만들 시간을 주고 8초에 오게 하고, 손님 간격도 8~13초로 늘렸습니다. 인내심 막대는 초록 → 주황 → 빨강으로 바뀌고, 거의 다 떨어지면 😤가 뜹니다.
- 의도: 다 될 때까지 기다리지 말고 되는 대로 먼저 건네는 플레이가 유리해지도록 했습니다.

**기술 스택**: three.js, JavaScript(단일 HTML), Web Audio API, GitHub Pages / 1인 개발(AI 바이브코딩 활용)
