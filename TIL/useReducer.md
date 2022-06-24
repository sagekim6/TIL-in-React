# `useReducer`

- `useState`처럼 스테이트를 만들어 관리할 수 있는 리액트 내장 hook이다. 즉, `useState`를 대체할 수 있다.
- `useState`는 컴포넌트 안에 정의해서 사용하지만 `useReducer`는 컴포넌트 외부에서 스테이트를 업데이트 시키는 부분을 분리하여 사용할 수 있다는 특징이 있다 .

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

1. `state` : 현재 스테이트
2. `dispatch` : `action`을 발생시키는 함수. 인자로 `type` 속성을 가진 `action` 객체를 넘겨준다.

   - `action`객체 : 상태 변화를 설명하는 객체

3. `reducer` : state와 action을 받아 새로운 state를 반환하는 함수
4. `initialState` : 스테이트 초기값

## `useReducer`를 사용하여 상태 변화 로직 분리하기

일반적으로 `useState`를 사용하여 상태 변화 함수를 작성하면 아래 코드와 같이 한 컴포넌트안에 많은 상태 변화 함수가 작성되는데 이는 컴포넌트를 무겁게 만들어서 별로 좋지않다.

```javascript
import { useState } from "react";

const Counter = () => {
  // useState 사용
  const [count, setCount] = useState(1);

  // 상태를 업데이트 시키는 함수가 한 컴포넌트에 여러개 정의된다.
  const add1 = () => {
    setCount(count + 1);
  };

  const add2 = () => {
    setCount(count + 10);
  };

  const add3 = () => {
    setCount(count + 100);
  };

  const add4 = () => {
    setCount(count + 1000);
  };

  const add5 = () => {
    setCount(count + 10000);
  };

  return (
    <div>
      {count}
      <button onClick={add1}>add 1</button>
      <button onClick={add2}>add 10</button>
      <button onClick={add3}>add 100</button>
      <button onClick={add4}>add 1000</button>
      <button onClick={add5}>add 10000</button>
    </div>
  );
};

export default Counter;
```

`count` 스테이트와 상태 변화(action)를 일으키는 `dispatch` 함수를 구조 분해 할당으로 받고 `useReducer`함수에는 `reducer` 함수와 `count` 스테이트의 초기값인 숫자 `1`을 넘겨준다.

```javascript
const [count, dispatch] = useReducer(reducer, 1);
```

`onClick` 함수에 `dispatch`함수를 실행시켜준다. 인자로는 `type` 속성을 가진 action 객체를 넣어준다. `type` 속성은 `reducer` 함수에서 switch-case문에서 사용된다.

```javascript
return (
  <div>
    {count}
    <button onClick={() => dispatch({ type: 1 })}>add 1</button>
    <button onClick={() => dispatch({ type: 10 })}>add 10</button>
    <button onClick={() => dispatch({ type: 100 })}>add 100</button>
    <button onClick={() => dispatch({ type: 1000 })}>add 1000</button>
    <button onClick={() => dispatch({ type: 10000 })}>add 10000</button>
  </div>
);
```

스테이트의 상태를 본격적으로 업데이트 시키는 함수가 `reducer` 함수이며 `dispath` 함수에 의해 실행된다.  
`reducer` 함수는 파라미터로 state와 action을 받고 action 객체의 `type` 속성으로 전달된 값을 통해 case가 실행되고 새로운 스테이트를 반환한다.

```javascript
// state와 action 객체를 파라미터로 받는다.
const reducer = (state, action) => {
  // action객체의 type속성값으로 case를 실행시킨다.
  switch (action.type) {
    case 1:
      return state + 1; // 항상 새로운 스테이트를 반환한다.
    case 10:
      return state + 10;
    case 100:
      return state + 100;
    case 1000:
      return state + 1000;
    case 10000:
      return state + 10000;
    default:
      return state;
  }
};
```

### 전체 코드

```javascript
import { useReducer } from "react";

const reducer = (state, action) => {
  switch (action.type) {
    case 1:
      return state + 1;
    case 10:
      return state + 10;
    case 100:
      return state + 100;
    case 1000:
      return state + 1000;
    case 10000:
      return state + 10000;
    default:
      return state;
  }
};

// useState를 사용한 코드와는 다르게 Counter 컴포넌트가 훨씬 가벼워졌다.
const Counter = () => {
  const [count, dispatch] = useReducer(reducer, 1);

  return (
    <div>
      {count}
      <button onClick={() => dispatch({ type: 1 })}>add 1</button>
      <button onClick={() => dispatch({ type: 10 })}>add 10</button>
      <button onClick={() => dispatch({ type: 100 })}>add 100</button>
      <button onClick={() => dispatch({ type: 1000 })}>add 1000</button>
      <button onClick={() => dispatch({ type: 10000 })}>add 10000</button>
    </div>
  );
};

export default Counter;
```
