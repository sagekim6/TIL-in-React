# State 끌어올리기(State Lifting)

상위 컴포넌트가 하위 컴포넌트로 전달한 `Props`나 `state`는 <b>읽기 전용</b>이다. 이러한 흐름을 <u>단방향 데이터 흐름</u>이라고 한다.

<b>단방향 데이터 흐름은</b>

1. 코드의 흐름을 알기 쉽고
2. 유지보수를 편리하게 한다는 장점이 있다.

하지만 `state`는 상위 컴포넌트에서 <b>setter 함수를 전달 받아</b> 하위 컴포넌트에서 상태값을 변경할 수 있는데 이것을 <u>'State Lifting'</u>이라고 한다.

```javascript
// 쓸데없지만 이해를 위한 간단한 예시
import React from "react";
import { useState } from "react";

// 숫자를 1씩 증가시키는 버튼이 있는 하위 컴포넌트
function IncreaseBtn({ num, onClick }) {
  const handleNumChange = () => onClick(num + 1); // prop으로 전달받은 setter 함수를 이벤트 함수안에서 사용

  return (
    <>
      <button onClick={handleNumChange}>Increase</button>
    </>
  );
}

// 상위 컴포넌트
function App() {
  const [num, setNum] = useState(0);

  return (
    <div>
      <div>{num}</div>
      <IncreaseBtn onClick={setNum} num={num} />
    </div>
  );
} // onClick prop으로 setter 함수인 setNum 함수를 전달하여 하위 컴포넌트에서 num state값을 변경가능하게 한다.

export default App;
```
