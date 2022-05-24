# State(스테이트)

- react 패키지에서 `useState` 함수를 import해준다.
- 리액트에서 변수 같은 역할을 한다. 즉, 데이터를 저장한다.

```javascript
// import 해주기
import { useState } from "react";

// ... 다른 코드들

const [num, setNum] = useState(1);
```

useState 함수는 파라미터로 초기값을 전달받고 배열의 형태로 요소 두개를 리턴한다. 보통 배열의 디스트럭처링 문법으로 코드를 작성한다.

- `0`: State 값. 현재 스테이트 값을 나타낸다. useState 함수를 호출할 때 전달한 초기값(ex. `1`)을 가진다.
- `1`: setter 함수. State값을 setter 함수를 통해 바꿔준다. 일반적으로 이름앞에 set을 붙인다.  
  -> setter 함수를 통해 state 값을 변경해 주기 때문에 변수를 선언할 때 const 키워드를 써준다.

```javascript
// import 해주기
import { useState } from "react";

// ... 다른 코드들

const [num, setNum] = useState(1);
// num state의 초기값은 1이 된다.

function changeNum() {
  setNum(3); // setter 함수인 setNum 함수를 이용해 num state의 값을 3으로 변경
}
```

### 스테이트가 업데이트되면 리액트가 알아서 화면을 새로 랜더링 한다.

```javascript
import React from "react";
import { useState } from "react";

// 버튼을 누르면 1씩 증가하는 컴포넌트
function App() {
  const [count, setCount] = useState(0);
  // count의 초기값은 0

  const increase = () => {
    setCount(count + 1); // setCount 함수를 이용해 1씩 더해준다
  };

  return (
    <>
      <div>{count}</div>
      <button onClick={increase}>증가</button>
    </>
  );
}

export default App;
```

스테이트가 업데이트될 때마다 App 함수가 다시 실행된다. 그래서 일반 변수를 사용하면 변수는 다시 초기값으로 돌아가는 문제가 발생한다.  
하지만 스테이트를 사용해서 setter 함수로 스테이트 값을 변경하면 <b>새로 변경한 값을 가지고 컴포넌트가 다시 실행</b>되는 방식으로 스테이트 값을 변경한다.

### state 값 안전하게 변경하기

setter 함수에 함수를 넘겨주는 방식으로 state값을 변경한다.  
count 값은 어디서나 변경될 수 있지만 아래 함수의 current 파라미터는 항상 현재 state값을 나타내기 때문에 안전하다.

```javascript
const increase = () => {
  // 현재 스테이트값을 바탕으로 다음 스테이트 값을 변경할 때 사용
  setCount((current) => current + 1);
};
```

## 참조형 State

- 자바스크립트의 자료형은 크게 기본형(primitive type)과 참조형(reference type)로 나눌 수 있다.  
  이때 참조형 값을 state의 기본값으로 넘겨줄 때 주의해야 할 부분이있다.

```javascript
import { useState } from "react";

// ... 다른 코드들

const [history, setHistory] = useState([]); // 초기값으로 참조형인 빈 배열을 넘겨줌
history.push(5);
setHistory(history);

// ... 다른 코드들
```

여기서 `history`는 배열 값 자체를 가지고 있는게 아니라 해당 배열의 주소값을 참조하고 있다. 때문에 push 메소드로 배열안에 숫자 5를 넣어도  
즉, 배열 안의 요소를 변경했다고 해도 결과적으로 `history`가 참조하는 주솟값은 변경되지 않아서 상태(State)가 바뀌었다고 판단하지 않는다.

그래서 참조형 State를 활용할 때는 반드시 새로운 참조형 값을 만들어 state를 변경해야 한다.
