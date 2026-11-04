# 김성연의 미니게임 (Mini Games)

일상의 행동 하나를 손맛으로 옮긴 3D 캐주얼 웹게임 모음입니다.
설치 없이 휴대폰과 PC 브라우저에서 바로 할 수 있습니다.

**▶ 게임 목록:** https://lalunru.github.io/mini-games/

| 게임 | 장르 | 한 줄 소개 | 플레이 | 만든 이야기 |
| --- | --- | --- | --- | --- |
| 막내의 회식 | 3D 캐주얼 | 삼겹살을 굽고 잘라 부장님 앞접시를 채워요 | [플레이](https://lalunru.github.io/mini-games/hoesik/) | [README](./hoesik/README.md) |
| 귤 까기 장인 | 3D 캐주얼 | 모양 틀대로 귤을 까고, 펼쳐서 확인해요 | [플레이](https://lalunru.github.io/mini-games/gyul/) | [README](./gyul/README.md) |
| 붕어빵 노점 | 3D 요리 경영 | 밀가루부터 반죽해서 손님 주문대로 구워 팔아요 | [플레이](https://lalunru.github.io/mini-games/bungeo/) | [README](./bungeo/README.md) |

각 게임 폴더의 README에 기획 의도, 조작법, 플레이 테스트 피드백을 반영한 밸런스 튜닝 기록이 있습니다.

## 폴더 구조

```
mini-games/
├── index.html        게임 목록 첫 화면
├── assets/           목록용 썸네일
├── hoesik/           막내의 회식
├── gyul/             귤 까기 장인
└── bungeo/           붕어빵 노점
```

각 게임은 `index.html` 파일 하나로 되어 있습니다. 밸런스 수치는 파일 위쪽 `CONFIG`에서 바꿀 수 있습니다.

## 새 게임 추가하기

1. 새 폴더를 만들고 게임의 `index.html`을 넣습니다.
2. 첫 화면 `index.html`의 카드 하나를 복사해서 제목, 설명, 폴더 이름을 바꿉니다.
3. 썸네일은 `assets/` 폴더에 넣습니다 (가로 640, 세로 400 권장).

## 기술

three.js, JavaScript (빌드 과정 없는 단일 HTML), Web Audio API, GitHub Pages
1인 기획과 개발 (AI 바이브코딩 활용)
