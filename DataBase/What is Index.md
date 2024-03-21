# Index란?
- **정렬해 놓은 컬럼의 사본**
- 컴퓨터가 빠르게 데이터를 찾을 수 있게 도와줄 수 있는 것.
- 기본적으로 데이터들을 따로 정렬해 놓은 데이터를 생성하여 쉽게 검색 할 수 있도록 만든 것.
- 단점은 Index를 생성해야 하기 때문에 DB의 용량을 추가적으로 먹는다는 것.

### 설명

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/3b48ba18-86ae-4f29-8f86-ce2eaeb98b2c)

만약 위와 같은 상황에서 특정 숫자를 검색해야 할 경우 Index가 없다면 1부터 시작해서 특정 숫자를 검색해야 하고, Database는 최악의 경우 100번 검색해야 한다. 
하지만, Index를 부여한다면 중간인 50을 기준으로 크거나 같은쪽으로 반씩 소거하면서 검색하게 된다. 이러한 경우 최악의 경우가 Index를 부여했을 경우보다 적어지기 때문에 검색 속도에 향상을 줄 수 있다.

### 추가 설명
기본적으로 Java들은 Array, Linked List와 같이 배열형태로 정렬을 한다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/81ac8c79-a3e2-443e-b895-9b50116687f1)

하지만 DB의경우 아무렇게나 일렬로 뿌려진 데이터를 정렬시키는 방식이 아닌 Binary Search Tree구조의 형태로 다음과 같이 데이터를 정렬한다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/a0d2262f-9a13-48d0-84b3-60eb60c0e851)

위의 Binary Search Tree의 경우는 B-Tree로 성능을 개선 시킬 수 있다.
B-Tree는 Node에 데이터를 한개씩 두어 절반씩 소거하는 방법이 2/3씩 소거 할 수 있게 해준다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/695ef028-4e6d-4185-a9c7-902b20334de7)

B-Tree도 성능을 개선 시킬 수 있는데, 해당 Tree이름은 B+Tree라고 부른다.
B+Tree는 Node마다 Data를 두는 것이 아닌 Data의 조건문 같은 가이드를 주고 최 하단에 데이터를 일열로 배치하는 구조를 사용한다.
또한 최 하단의 배치한 배열의 덩어리를 가로로 묶어 범위 검색에서 효과적으로 작동 할 수 있게 만들어 줄 수 있다.

![image](https://github.com/SubiYoon/SubiYoon.github.io/assets/117332903/696132a8-c14c-495e-af2f-938fcc415cac)
