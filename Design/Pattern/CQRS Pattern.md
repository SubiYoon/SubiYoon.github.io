---
tags:
  - Design
  - Pattern
  - CQRS
---
## CQRS Pattern??
명령(Command)과 질의(Qurey)의 책임(Responsebility)을 분리(Segregation)한다는 뜻의 약자를 따서 만든 패턴입니다.

쓰기를 위한 데이터 모델(Write Model)과 읽기를 위한 데이터 모델(Read Model)을 분리하는 패턴이라고 할 수 있는 것이지요.

그러면 명령(Command)과 질의(Qurey)란 무엇을 뜻하는 걸까요??

간단하게 말하게 표현해 보겠습니다.
* Command : 시스템의 상태를 변경(Create, Update, Delete)하는 작업
* Query : 시스템의 상태를 조회(Read)하는 작업

## CQRS는 왜 사용할까??
데이터를 쓰는 빈도보다 데이터를 읽는 빈도가 훨씬 높은 경우 쓰기를 위한 데이터 모델을 데이터 조회에 그대로 사용하는 경우 정규화된 테이블에 대한 조인 연산이 필요하여 성능에 부정적인 영향을 미칠 수 있습니다.
이때 읽기를 위한 간소화된 데이터 모델을 별도로 구성하면 성능적인 이점을 얻을 수 있습니다.

명령(Command)과 질의(Query)에 대한 데이터 모델 및 아키텍처를 별도로 최적화 할 수 있는것이죠.
* 각자의 데이터 모델 및 서로 다른 DataBase 사용 가능(MAS or 분산에서 활용 가능)
* 조회 성능 개선

## CQRS의 구현방법 - 단일 DB
* DataBase는 분리하지 않고 모델만 분리해서 사용하는 방법입니다.
* 단순하게 구현/적용이 가능하지만, 단일 DB 사용에 따른 확장성 제한을 개선하진 못합니다.
  
![R1280x0.png](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/R1280x0.png?raw=true)

## CQRS의 구현방법 - 다중 DB
* 쓰기 전용과 읽기 전용 DB를 분리하고 동기화 시킬 수 있습니다.
* 단일 DB에 비해 확장성의 문제는 해결이 가능하지만, 신뢰성은 보장이 되지 않을 수 있습니다.
  
![R1280x0-2.png](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/R1280x0-2.png?raw=true)

## CQRS 구현방법 - 이벤트 소싱
* 이벤트소싱이란 모든 명령(Command)을 이벤트 형태로 별도의 이벤트 저장소에 저장합니다.
* 이 명령들은 자연스럽게 다시 사용할 수 있습니다.
* 강한 일관성 및 실시간 업데이트가 필요한 시스템에는 적합하지 않을 수 있습니다.

![R1280x0-3.png](https://github.com/SubiYoon/SubiYoon.github.io/blob/main/Attached%20File/R1280x0-3.png?raw=true)
