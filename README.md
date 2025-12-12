# 2048 게임

React를 사용하여 구현한 2048 퍼즐 게임입니다. 커스텀 훅과 게임 로직 모듈화를 통해 구조화된 코드로 작성했습니다.

## 기술 스택

- React 19.1.1: 컴포넌트 기반 UI 라이브러리
- React Hooks: useState, useEffect, useCallback을 활용한 상태 관리
- Create React App: 프로젝트 초기 설정 및 빌드 도구
- Jest & React Testing Library: 단위 테스트 및 컴포넌트 테스트

## 주요 기능

게임 플레이
- 화살표 키를 통한 4방향 이동 (위, 아래, 왼쪽, 오른쪽)
- 타일 병합 시 점수 증가
- 랜덤 타일 생성 (90% 확률로 2, 10% 확률로 4)
- 2048 타일 달성 시 승리
- 더 이상 이동 불가능할 때 게임 오버

Undo 기능
- 최대 2회까지 이전 상태로 되돌리기
- Undo 사용 횟수 표시
- 게임 오버 상태에서는 Undo 비활성화

게임 상태 저장
- localStorage를 통한 자동 저장
- 페이지 새로고침 시에도 게임 상태 유지
- 게임 상태, 점수, Undo 스택 모두 저장

게임 리셋
- 새 게임 시작 시 초기 상태로 리셋
- Undo 스택 및 사용 횟수 초기화

## 구현 세부사항

게임 로직은 gameLogic 모듈로 분리하여 구현했습니다. moveMapIn2048Rule 함수는 방향에 따라 맵을 회전시켜 moveLeft 함수로 통일된 로직으로 처리합니다. 이는 코드 중복을 방지하고 유지보수성을 높입니다.

moveRowLeft 함수는 reduce를 사용하여 행을 왼쪽으로 이동시키고 병합합니다. null 셀을 건너뛰고, 같은 값의 타일을 만나면 병합하며, 다른 값이면 순서대로 배치합니다.

게임 상태 관리는 use2048Game 커스텀 훅에서 처리합니다. useState로 게임 상태를 관리하고, useEffect를 통해 localStorage에 자동 저장합니다. useCallback을 사용하여 함수 메모이제이션을 통해 불필요한 리렌더링을 방지합니다.

키보드 이벤트 처리는 window.addEventListener를 통해 구현했습니다. ArrowUp, ArrowDown, ArrowLeft, ArrowRight 키를 감지하여 해당 방향으로 이동을 처리합니다. 게임이 진행 중이 아닐 때는 입력을 무시합니다.

Undo 기능은 undoStack 배열에 이전 상태를 저장하여 구현했습니다. 이동이 발생할 때마다 현재 상태를 스택에 저장하고, Undo 시 마지막 상태를 복원합니다. 최대 2회까지만 Undo할 수 있도록 undoCount로 제한합니다.

게임 오버 체크는 checkGameOver 함수에서 처리합니다. 모든 셀이 채워져 있고, 모든 방향으로 이동이 불가능할 때 게임 오버로 판단합니다. 각 방향으로 이동을 시도하여 변화가 있는지 확인합니다.

컴포넌트 구조는 Game, Board, Tile로 분리했습니다. Game 컴포넌트는 게임 상태와 컨트롤을 관리하고, Board는 4x4 그리드를 렌더링하며, Tile은 개별 타일을 표시합니다.

## 프로젝트 구조

```
2048-game/
├── src/
│   ├── App.js
│   ├── components/
│   │   ├── Game.js
│   │   ├── Board.js
│   │   └── Tile.js
│   ├── hooks/
│   │   └── use2048Game.js
│   ├── gameLogic/
│   │   └── index.js
│   └── index.js
└── package.json
```
