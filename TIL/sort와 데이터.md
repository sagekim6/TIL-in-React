# `sort()`로 데이터 다루기

데이터를 정렬할 수 있다.

1. <b>인자를 아무것도 전달하지 않으면</b> 기본적으로 <u>유니코드</u>에 정의된 문자열 순서에 따라 정렬된다.  
   그래서 일반적인 오름차순이나 내림차순으로 정렬되지 않는다.

```javascript
const letters = ["D", "C", "E", "B", "A"];
const numbers = [1, 10, 4, 21, 36000];

letters.sort();
numbers.sort();

console.log(letters); // [ 'A', 'B', 'C', 'D', 'E' ]
console.log(numbers); // [ 1, 10, 21, 36000, 4 ]
```

2. 오름차순, 내림차순으로 정렬하고 싶을 때  
   인자를 작성해주면된다. 또한 <b>원본 배열의 요소를 정렬</b>하기 때문에 한 번 정렬하면 정렬 전의 순서로 다시 되돌릴 수 없으니 주의해야한다.

```javascript
const numbers = [1, 10, 4, 21, 36000];

// 오름차순 정렬
numbers.sort((a, b) => {
  return a - b;
});

console.log(numbers); // [ 1, 4, 10, 21, 36000 ]
```

```javascript
const numbers = [1, 10, 4, 21, 36000];

// 내림차순 정렬
numbers.sort((a, b) => {
  return b - a;
});

console.log(numbers); // [ 36000, 21, 10, 4, 1 ]
```

## 최신순, 베스트순으로 데이터 정렬하기

```javascript
import { useState } from "react";
import ReviewList from "./components/ReviewList";
import items from "./mock.json";

function App() {
  const [order, setOrder] = useState("createdAt");

  // sort 함수로 데이터 정렬
  const sortedItems = items.sort((a, b) => b[order] - a[order]);

  // setOrder()를 사용해 문자열 변경
  const handleBestClick = () => setOrder("rating");
  const handleNewestClick = () => setOrder("createdAt");

  return (
    <div>
      <div>
        <button onClick={handleNewestClick}>최신순</button>
        <button onClick={handleBestClick}>베스트순</button>
      </div>
      <ReviewList items={sortedItems} />
    </div>
  );
}
```
