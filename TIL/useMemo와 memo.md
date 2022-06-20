# `useMemo` VS `React.memo`

<b>공통점</b>: 둘다 props가 변하지않고 동일하다면 이전에 메모이즈된 결과를 반환한다.

## `React.memo()` 함수
- `React.memo()`는 고차 컴포넌트(Higher Order Component)이다.  
  -  고차 컴포넌트(Higher Order Component) : 고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수이다.  
- `React.memo()`는 말 그대로 <u>함수</u>이며 컴포넌트를 인자로 받아 새로운 컴포넌트를 리턴한다.  

```javascript
const EnhancedComponent = higherOrderComponent(WrappedComponent);
// 고차 함수로 다른 컴포넌트를 래핑해서 사용하는 형태
```

```javascript
import { useState } from "react";
import { memo } from "react"; // 1. memo 함수를 import 해준다

function Button({ text, changeValue }) {
  return (
    <>
      <button onClick={changeValue}>{text}</button>
    </>
  );
}

const MemoBtn = memo(Button); // 2. Button 컴포넌트를 memo 함수로 래핑

// 스테이트가 변하면서 App 컴포넌트는 다시 랜더링 된다.
function App() {
  const [value, setValue] = useState("Save Changes");
  const changeValue = () => {
    setValue("Revert Changes");
  };

  // 3. 하지만 래핑한 MemoBtn을 컴포넌트로 사용시 -> 해당 버튼은 리랜더링되지 않는다
  return (
    <>
      <h1>Hello World</h1>
      <Button text={value} changeValue={changeValue} />
      <MemoBtn text="bye~" />
    </>
  );
}

export default App;
```

## `useMemo(callback, [dependencies])` Hook

- `useMemo`는 Memoized value를 리턴하는 <u>리액트 Hook이다.</u>  
  
<b>What is Memoized value?</b> - 함수에 특정값의 호출결과를 기억해 놓은 값  
- <b>Memoization</b>: 최적화를 위해 사용하는 개념이다. 비용이 높은 즉, 연산량이 많은 함수의 호출 결과를 저장해놨다가 같은 입력값으로  
  함수를 호출했을 때 함수를 새로 호출하지 않고 이전에 저장해 놨던 호출 결과를 바로 반환하는 것이다.  
  -> 불필요한 연산을 줄일 수 있고 값을 받는 시간도 빨라진다.  
  
- 의존성 배열의 값이 변하게 되면 콜백이 실행되고 그렇지 않으면 기존 함수의 결과값을 그대로 반환한다.
- 의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행되기 때문에 의존성 배열은 필수이며 의존성 배열이 빈 배열인 경우, 컴포넌트 마운트(생성) 시에만 호출 된다.  
- 렌더링이 일어나는 동안 실행된다.  
  -> 렌더링이 일어나는 동안 실행되선 안되는 일에 useMemo를 사용해선 안 된다.  

1. `useMemo`를 임포트 해준다
```javascript
import { useMemo } from 'react'; // 1. useMemo를 임포트 해준다
```  

2. `useMemo()`의 첫 번째 인자로 메모이즈 할 함수를 작성하고, 의존값을 배열에 넣어준다.  
```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```  

```javascript
import React,{useMemo} from "react";
import "./styles.css";

const getNumber = (number) => {
  console.log("숫자가 변동되었습니다.");
  return number;
};

const getText = (text) => {
  console.log("글자가 변동되었습니다.");
  return text;
};

const ShowState = ({ number, text }) => {
  const showNumber = useMemo(()=>getNumber(number),[number]);
  const showText = useMemo(()=>getText(text),[text]);

  return (
    <div className="info-wrapper">
      {showNumber} <br />
      {showText}
    </div>
  );
};

export default ShowState;
```
