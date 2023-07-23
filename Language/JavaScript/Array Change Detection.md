# Array Change Detection

### Mutation Methods (돌연변이 메서드)

- 호출된 원래 배열을 변형시키는 메서드
- template태그 안에서 사용 할 수 있다.

```jsx
var arr = [1, 2, 3];
```

- 종류
    - **push( )**
        
        → 새로운 데이터를 추가시키는 메서드
        
        → 배열의 크기를 return
        
        ```jsx
        arr.push('new') //return: 4, arr: [1, 2, 3, "new"]
        ```
        
    - **pop( )**
        
        → 배열의 마지막 요소를 제거
        
        → 제거된 요소를 return
        
        ```jsx
        arr.pop(); //return: 3, arr: [1, 2]
        ```
        
    - **shift( )**
        
        → 배열의 첫번째 요소를 제거
        
        → 제거된 요소를 return
        
        ```jsx
        arr.shift(); //return: 1, arr: [2, 3]
        ```
        
    - **unshift( )**
        
        → 배열에 요소를 추가하고 이를 맨앞의 요소에 삽입
        
        → 배열의 크기를 return
        
        ```jsx
        arr.unshift('new'); //return: 4, arr: ['new', 1, 2, 3]
        ```
        
    - **splice( )**
        
        → 배열 임의의 위치에 요소 추가 제거
        
        → 해당 배열을 return
        
        ```jsx
        /*
        start - 수정할 배열 요소의 index
        deleteCount - 삭제할 요소 개수, 제거하지 않을 경우 0
        el - 배열에 추가될 요소
        */
        arr.slice(start, deleteCount, ...el);
        
        var arr = [1, 5, 7];
        arr.slice(1, 0, 2, 3, 4); //[], arr: [1, 2, 3, 4, 5, 7]
        arr.slice(1, 2);          //[2, 3], arr: [1, 4, 5, 7]
        
        ```
        
    - **sort( )**
        
        → 내부 요소를 정렬하는데 사용
        
        - 기본적으로 문자열로 변경후에 정렬하기에 오류가 발생할 수 있다.
        - 따라서 함수를 이용하여 이를 해결한다.
        - 순차적으로 a, b 인자를 비교하여 정렬한다.
        
        ```jsx
        var arr = [11, 1, 115, 42, 12];
        arr.sort(); //arr: [1, 11, 115, 12, 42] <-문제 있음
        
        //함수를 넣은 방법
        //결과값이 0보다 작으면 a가 낮은 색인으로 정렬
        //결과값이 0이면 a, b의 순서를 변경하지 않음
        //결과값이 0보다 크면 b가 낮은 색인으로 정렬
        arr. sort(function(a, b) {return a-b;}); //arr: [1, 11, 12, 42, 115]
        
        //알파벳 정렬
        var arr = [
          {name: "Dinah"},
          {name: "Jack"},
          {name: "Alice"},
          {name: "Ada"}
        ];
        
        // name 프로퍼티의 값을 기준으로 알파벳 순으로 정렬
        arr.sort(function(a, b) { return a.name > b.name });
        ```
        
    - **reverse( )**
        
        → 배열 요소의 순서를 반전시키는데 사용
        
        ```jsx
        var arr = [1, 3, 5, 7];
        arr.reverse();  //arr: [7, 5, 3, 1]
        ```
        
        ### Replacing an Array
        
        - 원래 배열을 변경하지 않고 새 배열을 반환하는 비변환 메서드
        - DOM의 재사용을 극대화하기 위한 방법
        - 종류
            - **filter( )**
                
                → 필터링 기능
                
                ```html
                <script setup>
                const sets = ref([
                  [1, 2, 3, 4, 5],
                  [6, 7, 8, 9, 10]
                ])
                
                function even(numbers) {
                  return numbers.filter((number) => number % 2 === 0)
                }
                </script>
                
                <template>
                  <ul v-for="numbers in sets">
                    <li v-for="n in even(numbers)">{{ n }}</li>
                  </ul>
                </template>
                ```
                
            - **concat( )**
                
                → 데이터를 합치는 기능
                
                ```jsx
                var a = [1, 2, 3];
                var b = [4, 5, {name: 'Kevin}];
                var c = a.concat(b);
                console.log(c); // [1, 2, 3, 4, 5, {name: 'Kevin'}]
                
                b[1] = 'power';
                b[2].name = 'Bob';
                console.log(c); // [1, 2, 3, 4, 5, {name: 'Kevin'}]
                console.log(b); // [4, 'power', {name: 'Bob'}]
                ```
                
            - **slice(${number}, ${number} )**
                
                → parameter를 하나도 넣지 않을 경우 데이터 전체를 복사해서 가져옴
                
                ```jsx
                var nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];
                
                nums.slice(); // nums와 같다.
                ```
                
                → parameter를 하나 넣을 경우 해당 index이후 데이터를 가져옴
                
                ```jsx
                var nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];
                
                nums.slice(10) //nums: [10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
                ```
                
                → parameter를 두개 넣을 경우 해당 index ~ 나중 index 이전까지 가져옴
                
                ```jsx
                var nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19];
                
                nums.slice(5, 10); //nums: [5, 6, 7, 8, 9]
                ```