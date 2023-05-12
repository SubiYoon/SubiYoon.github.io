# [변수 + “구분자”] = value
<br>
<br>

# this[변수 + “구분자”] = value

- 현재 스크립트에서 새로운 변수 값을 할당하여 해당 value 값을 설정

# 변수1[변수2 + “구분자”] = value

- 선언되어 있는 변수(List 형식)에 Set or Map과같이 새로운 key를 할당해 value값을 설정

```jsx
var test = {};

var transId = "val";

test["_label"] = "test1";

console.log(test);
// {_label : "test1"}

test[transId + "_label"] = "test2";

console.log(test);
// {_label : "test1", val_label : "test2"}

transId = "sample";

test[transId + "_label"] = "sample";

console.log(test);
// {_label : "test1", val_label : "test2", sample_label : "sample"}

test["val_label"] = "Edit";

console.log(test);
// {_label : "test1", val_label : "Edit", sample_label : "sample"}
```