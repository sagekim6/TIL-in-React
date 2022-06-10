# `useMemo` VS `React.memo`

<b>공통점</b>: 둘다 props가 변하지않고 동일하다면 이전에 메모이즈된 결과를 반환한다.

## `React.memo()` 함수

- `React.memo()`는 말 그대로 함수이며 컴포넌트를 인자로 받아 새로운 컴포넌트를 리턴한다.

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

- `useMemo`는 메모이즈된 값을 return하는 hook이다.
- 두번째 인자인 의존값(dependencies)에 들어간 값이 변경되면 값을 재계산 한다.
