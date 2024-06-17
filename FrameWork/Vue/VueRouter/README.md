---
tags:
  - FrameWork
  - Vue
  - Router
---
# LifeCycle

![Vue Router LifeCycle Hook](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/Vue%20Router%20LifeCycle%20Hook.jpg?raw=true)

### beforeRouteLeave
기존에 존재하던 컴포넌트가 제거되기 전에 이 훅을 호출
주로 문서(게시글 등)를 저장하지 않은 상태로 다른 곳으로 이동하는 것을 방지하는데 사용

### beforeEach
기존의 컴포넌트가 제거된 후 새로운 네비게이션이 시작될 때 이 훅을 호출
전역으로 사용할 로직을 명시

### beforeRouteUpdate
컴포넌트를 재사용 할 경우에만 발생하는 훅
예를 들면 `/board/page/1`에서 `/board/page/2`로 게시판의 페이지를 이동 할 경우

### beforeEnter
이동하려는 라우트에 진입하기 전 호출되는 훅
`beforeEach`와 달리 라우트마다 각각 다르게 가드를 지정할 수 있음

### beforeRouteEnter
새로운 컴포넌트를 만들기 전 호출되는 훅
아직 컴포넌트를 만들기 전이므로 컴포넌트를 this로 접근할 수 없음
객체에 접근하고자 할 경우 세 번째 인자인 `next`함수에 콜백을 이용하여 접근가능
`next`함수에 콜백을 걸어뒀다면, 돔이 업데이트된 후 실행됨

### beforeResolve
네비게이션 작업을 완료하기 전에 호출되는 훅
`beforeEach`와 마찬가지로 전역으로 사용되는 전역 가드

### afterEach
네비게이션 작업이 완료된 후 호출되는 훅
네비게이션 작업에 영향을 끼치지 않음