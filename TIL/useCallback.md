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
