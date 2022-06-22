# `useCallback()`

- 메모이제이션된 <b>콜백을 반환</b>한다. `useMemo`와 사용법이 비슷하다.
  - `useMemo`도 메모이제이션하긴 하지만 `useCallback`은 함수를 반환하고 <b>`useMemo`는 값을 반환</b>하는 차이가 있다.
- 컴포넌트가 렌더링될 때마다 그 안에 선언되어있는 변수나 함수들은 다시 초기화된다. 즉, 계속 새로운 변수나 함수를 만들어 낸다.

```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

- `a` 또는 `b`값이 바뀌면 바뀐값을 적용해 새로운 콜백을 반환한다.
<hr />

```javascript
import React from "react";
import { useState, useCallback } from "react";

// Light 컴포넌트
function Light({ room, on, toggle }) {
  console.log({ room, on });
  return (
    <button onClick={toggle}>
      {room} {on ? "💡" : "⬛"}
    </button>
  );
}

// Light 컴포넌트를 memo로 래핑
Light = React.memo(Light);

// 침실, 주방, 욕실의 스위치 중앙 제어
function App() {
  const [masterOn, setMasterOn] = useState(false);
  const [kitchenOn, setKitchenOn] = useState(false);
  const [bathOn, setBathOn] = useState(false);

  const toggleMaster = () => setMasterOn(!masterOn);
  const toggleKitchen = () => setKitchenOn(!kitchenOn);
  const toggleBath = () => setBathOn(!bathOn);

  // Light 컴포넌트를 3개 만듬
  return (
    <>
      <Light room="침실" on={masterOn} toggle={toggleMaster} />
      <Light room="주방" on={kitchenOn} toggle={toggleKitchen} />
      <Light room="욕실" on={bathOn} toggle={toggleBath} />
    </>
  );
}

export default App;
```

위의 코드를 보면 `Light` 컴포넌트를 `memo` 함수로 감싸줬기 때문에 props가 변하지 않으면 리렌더링되지 않아야 하지만  
버튼을 클릭하면 콘솔에는 변경되지 않는 `Light` 컴포넌트까지 전부 출력되는 것을 볼 수 있다.
이유는 `toggle` prop 때문인데 바로 함수를 전달하고 있기 때문이다.  
자바스크립트에서 함수는 객체이기 때문에 `App`컴포넌트가 리렌더 되면서 함수가 재정의 되고 이 과정에서 함수의 참조값이 바뀌면서 결국 `toggle` prop의 값이 바뀌게 되는 것이다.

그래서 이런 경우에 `useCallback`을 사용해준다.

```javascript
import React from "react";
import { useState, useCallback } from "react";

function Light({ room, on, toggle }) {
  console.log({ room, on });
  return (
    <button onClick={toggle}>
      {room} {on ? "💡" : "⬛"}
    </button>
  );
}

Light = React.memo(Light);

function App() {
  const [masterOn, setMasterOn] = useState(false);
  const [kitchenOn, setKitchenOn] = useState(false);
  const [bathOn, setBathOn] = useState(false);

  // toggle prop으로 넘겨주는 이벤트 함수들에 useCallback을 사용해준다.
  const toggleMaster = useCallback(() => setMasterOn(!masterOn), [masterOn]);
  const toggleKitchen = useCallback(
    () => setKitchenOn(!kitchenOn),
    [kitchenOn]
  );
  const toggleBath = useCallback(() => setBathOn(!bathOn), [bathOn]);

  return (
    <>
      <Light room="침실" on={masterOn} toggle={toggleMaster} />
      <Light room="주방" on={kitchenOn} toggle={toggleKitchen} />
      <Light room="욕실" on={bathOn} toggle={toggleBath} />
    </>
  );
}

export default App;
```

`useCallback`을 사용한 부분을 따로 보면

```javascript
const toggleMaster = useCallback(() => setMasterOn(!masterOn), [masterOn]);
```

의존성 배열에 `masterOn`을 넣어 해당 값이 바뀌면 등록한 콜백이 실행되게 `useCallback`을 사용하였다. 이제는 버튼을 클릭하면 클릭한 요소만 콘솔에 찍히는 것을 볼 수 있다.
