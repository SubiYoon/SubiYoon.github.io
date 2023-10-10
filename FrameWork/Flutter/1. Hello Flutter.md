# 1. Hello Flutter

## 1-1) Hello World

### Widget

- 마치 레고 블럭 같은 느낌
- 위젯들을 모두 모아 하나의 웹 페이지를 만듬
- 위젯을 만든다는건 class를 만든다는 것과 비슷

### runApp([App()])

- App이라는 class를 만들어 실행
- App은 StatelessWidget를 상속받아 build메소드를 구현
- 구현한 build메소드를 CupertinoApp(IOS)와 MaterialApp(Android) 중 return 할 것을 선택
    - 커스터마이즈를 하고 싶어도 기본 UI설정과 재료를 선택해 주어야 하기 때문에 선택해야함

### scaffold

- 상단 버튼, 중앙 정렬, bottom, tab, bar 등을 해줌