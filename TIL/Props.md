# Props

- 리액트에서 컴포넌트에 속성을 지정할 수 있는데 이렇게 지정한 속성을 `Props`라고 한다.
- `Props`는 Properties의 약자로 지정된 속성이 <b>하나의 객체로</b> 모여서 컴포넌트를 정의한 함수의 첫 번째 파라미터로 전달된다.
  - 디스트럭처링 문법을 사용해서 `props.`을 생략할 수도 있다.
- 상위 컴포넌트가 하위 컴포넌트에 속성을 전달한다.  
  -> 하위 컴포넌트에선 Props는 읽기 전용값이다.

```javascript
import React from "react";

// <하위 컴포넌트>
// name과 color 속성을 부모 컴포넌트로 부터 전달 받는다.
function Greeting({ name, color }) {
  return (
    <div>
      <h1 style={{ color: color }}>Hello, {name}!</h1>
    </div>
  );
}

// <상위 컴포넌트>
function App() {
  return (
    <main>
      <Greeting name="Sage" color="pink" />
      <Greeting name="Sese" color="blue" />
      <Greeting name="Kim" color="grey" />
    </main>
  ); // Greeting 컴포넌트를 정의하고 그 안에 속성을 전달한다.
}

export default App;
```

## children

- Props에는 `{children}`이라는 특별한 프로퍼티가 있다.
- 컴포넌트를 단일 태그가 아니라 <b>여는 태그와 닫는 태그의 형태로 작성</b>하면 그 안에 담기는 내용은 `children`값에 담기게 된다.
- 단순히 텍스트만 작성해도 되고 컴포넌트 안에 컴포넌트를 작성할 수도 있다.

```javascript
// children props 전달
function Button({ children }) {
  return (
    <>
      <button>{children}</button>
    </>
  );
}

function App() {
  return (
    <>
      <h1>Hello World</h1>
      <Button>Click me!</Button> // 컴포넌트를 닫는 태그와 여는 태그로 작성하고
      <Button>Bye~</Button> // 그 안에 내용(텍스트)작성 -> 컴포넌트의 children
      인자로 전달
    </>
  );
}

export default App;
```

결론, 어떤 정보를 전달할 때는 일반적인 props의 속성값을 주로 활용하고, 화면에 보여질 모습을 조금 더 직관적인 코드로 작성하고자 할 때 children값을 활용하자

## React.memo() 함수

- 컴포넌트가 동일한 props로 동일한 결과를 랜더링한다면 memo()함수에 컴포넌트를 전달하여 래핑한다.  
  래핑된 컴포넌트는 다시 랜더링 되지 않고 마지막으로 랜더링된 결과를 재사용한다.  
  -> 불필요한 랜더링을 막을 수 있다.

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

  return (
    <>
      <h1>Hello World</h1>
      <Button text={value} changeValue={changeValue} />
      <MemoBtn text="bye~" /> // 3. 하지만 래핑한 MemoBtn을 컴포넌트로 사용시 ->
      해당 버튼은 리랜더링되지 않는다
    </>
  );
}

export default App;
```

### `memo()` 함수로 객체 최적화하기

객체는 안에 내용이 같더라도 참조값이 다르면 다른 값으로 본다(얕은 비교를 한다).  
그래서 객체를 최적화하려면 비교함수를 정의하여 인자를 넘겨주어야한다.

```javascript
import { useEffect, useState, memo } from "react";

// CounterA 컴포넌트
const CounterA = memo(({ count }) => {
  useEffect(() => {
    console.log("Counter A Updated: ", count);
  });

  return <div>{count}</div>;
});

// CounterB 컴포넌트
const CounterB = ({ obj }) => {
  useEffect(() => {
    console.log("Counter B Updated: ", obj.count);
  });

  return <div>{obj.count}</div>;
};

// 비교함수
const areEqual = (prevProps, nextProps) => {
  if (prevProps.obj.count === nextProps.obj.count) {
    return true;
  }
  return false;
};

// CounterB 컴포넌트와 비교함수를 memo 함수의 인자로 넘겨준다.
const MemoizedCounterB = memo(CounterB, areEqual);

function OptimizeTest() {
  const [count, setCount] = useState(1);
  const [obj, setObj] = useState({
    count: 1,
  });

  return (
    <div style={{ padding: 50 }}>
      <div>
        <h2>Counter A</h2>
        <CounterA count={count} />
        <button onClick={() => setCount(count)}>A button</button>
      </div>
      <div>
        <h2>Counter B</h2>
        <MemoizedCounterB obj={obj} /> <!-- memo함수가 할당된 변수를 컴포넌트로 사용한다. -->
        <button onClick={() => setObj({ count: obj.count })}>B button</button>
      </div>
    </div>
  );
}

export default OptimizeTest;
```
