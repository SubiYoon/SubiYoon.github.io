# Event Roof & Task Queue(Micro Task, Macro Task)

JS는 싱글 스레드 기반의 언어이고, JS 엔진은 하나의 호출 스택만을 사용한다.

이는 요청이 동기적으로 처리되어, 한 번에 한가지 일만 처리할 수 있음을 의미한다.

# Browser의 환경 구조
- **Node.js의 환경구조**

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/0109b3da-1ea9-4c84-9854-5e59ce94c1b7)

- **Browser의 환경구조**

![image 1](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/bfe08600-a445-42b3-96ba-c7c0acb32636)


### JavaScript Engine

- **Heap**
    
    → **객체들이 할당 되는 메모리**
    
    → 크기가 동적으로 변하는 값들의 참조 값을 가진다.
    
- **Call Stack**
    
    → 함수 호출 시, 실행 컨텍스트가 생성되며, 이러한 실행 컨텍스트들이 콜 스택을 구성한다.
    

### Web API or Browser API

- 웹 브라우저에 구현된 API
- DOM event, AJAX, Timer 등이 있다.

### Event Loop

- 콜 스택이 비어있따면, 테스크 큐에 있는 콜백 함수를 처리한다.

### Task Queue

- Event Loop는 하나이상의 TaskQueue를 가진다.
- TaskQueue는 Task의 Set이다.
- Event Loop가 Queue의 첫번째 Task를 가져오는 것이 아니다.
- TaskQueue에서 실행 가능한(runnable) 첫번째 Task를 가져오는 것!!
    - Task 중에서도 가장 오래된 Task를 가져온다.

# Event Loop

### Event Loop 동작 방식

- MacroTask Queue에서 가장 오래된 Task를 꺼내서 실행
- MicroTask Queue에 있는 모든 Task를 실행
- 렌더링 작업을 실행
- MacroTask Queue에 새로운 MacroTask가 나타날 때까지 대기한다.
- 위 동작을 반복한다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/11d04e9f-d0ae-4260-9353-6c01add72ff4)


### Event Loop의 필요성

- Brower와 Node.js에서 공통으로 제공한다.
- JavaScript는 싱글 쓰레드 기반의 언어이다.
- But!! JavaScript가 구동되는 환경(Node.js, Brower)는 여러 쓰레드가 사용된다.
- **여러 쓰레드가 사용되는 구동 환경이 JavaScript Engine과 연동하기 위해 사용되는 장치(=Event Loop)**
- 웹 사이트나 애플리케이션의 코드는 메인 쓰레드에서 실행되며, 같은 이벤트 루프를 공유한다.

### 메인 쓰레드의 역할

- 코드를 실행
- 이벤트들을 받고 실행
- 웹 컨텐츠를 렌더링하거나 페인팅

### Event Loop의 종류

- window event loop
    - 같은 Origin인 모든 윈도우는 윈도우 Event loof를 공유한다.
    - 모든 윈도우들은 동기적으로 소통할 수 있게 된다.
- worker event loop
    - window event loop와는 독립적으로 실행된다.
- worklet event loop

# MicroTask & MacroTask

![image 1](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/cc5747da-41b3-45ee-a940-0c63d2d24917)


- **MicroTask**들은 실행하면서 새로운 MicroTask를 Queue에 추가 할수도 있다.
    - **MicroTask**
        
        → **현재 수행 중인 작업이 끝난 뒤에 이어서 실행될 작업(콜백)을 말한다.**
        
        → MacroTask 보다 실행 우선순위가 높다.
        
        → **ex) Promise handler의 then( ), catch( ), finally( )와 같은 것 들**
        
- 새롭게 추가된 MricroTask도 Queue가 빌 때까지 계속해서 실행된다.
- **Event Loop는 MacroTask Queue에 있는 것을 실행시키기 시작할 때 있는 MacroTask만 실행시킨다.**
    - **MacroTask**
        
        → Call Stack작업이 끝난 후에 실행되는 작업
        
        → 스케쥴된 순서대로 실행된다.
        
        → **ex) JavaScript에서 setTimeout, setInterval, setImmediate(Node.js), requestAnimationFrame, 그 외 I/O 연산들**
        
- MacroTask가 추가한 MacroTask는 다음 Event Loop가 실행될 때까지 실행되지 않는다.
    - 추가도니 MacroTask는 추가된 MicroTask가 있으면 MicroTask가 실행될 때까지 실행되지 않는다??

### 공통점

- Call Stack에 있는 작업이 완료된 후에 실행되는 비동기 작업이다.
    - 동시에 두 종류의 작업이 대기중이라면, MicroTask가 우선순위를 갖는다.
- JavaScript 메은 쓰레드 밖에서 실행되어야 할 내용을 담고 있는 경우가 많다.
    - UI가 먹통이 되는 현상을 막귀 위해서는 필수적
        - 비동기 로드가 길어지면, UI는 응답이 불가능해진다.
- 웹, 앱의 응답성을 높이기 위해 사용

# Task Queue

- Task Queue는 MicroTask Queue(Event Queue)와 MacroTask Queue(Job Queue)로 나뉜다.
