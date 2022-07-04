### Prop Drilling 문제가 발생

리액트로 props를 넘겨주다 보면 오로지 하위 컴포넌트로 전달하기만 하는 props가 존재한다.  
해당 props는 컴포넌트 트리의 **한 부분에서 다른 컴포넌트로 이동하기 위해서만** 쓰이는데 이것을 <u>Prop Drilling</u>이라고 한다.

&rarr; 이 문제를 해결할 수 있는 방법이 바로 `Context` API를 사용하는 것이다.

# `Context` API

context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있다.  
보통 리액트는 단방향 흐름으로 데이터를 전달하는데 context를 이용하면 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있다.

모든 데이터를 가지고 있는 컴포넌트가 `<Provider />`라는 자식 컴포넌트에게 자신이 가지고 있는 모든 데이터를 전부 준다.  
이 `<Provider />` 컴포넌트는 자신의 자손 컴포넌트에 직통으로 데이터를 내려줄 수 있다. 그리고 `<Provider />` 컴포넌트가 공급하는 모든 데이터에 접근할 수 있는 하위 컴포넌트들의 영역을 문맥(context)이라고 표현한다.

### 1. `Context` 생성

```javascript
const MyContext = React.createContext(defaultValue);
```

### 2. Context Provider를 통한 데이터 공급

```javascript
<MyContext.Provider value={전역으로 전달하고자 하는 값}>
  // 하위 컴포넌트가 위치할 곳
</MyContext.Provider>
```

- `<MyContext.Provider>` 아래에 위치하는 컴포넌트들은 `value`로 전달되는 데이터를 직통으로 받을 수 있다.

### 3. `useContext` Hook을 사용해 컨텍스트 조회하기

```javascript
const value = useContext(MyContext);
```

- `useContext`의 인자로 생성한 컨텍스트를 넘겨준다. 컨텍스트를 `value` 변수에 담아서 사용한다.

### 간단한 예제

```javascript
import { useState } from "react";
import SubA from "./component/SubA";

// App 컴포넌트
function App() {
  const [text, setText] = useState("");

  return (
    <div className="App">
      <SubA text={text} onChange={setText} />
    </div>
  );
}

export default App;
```

```javascript
import SubB from "./SubB";

// Sub Component A
const SubA = ({ text, onChange }) => {
  return (
    <>
      <h2>Sub Component A</h2>
      <SubB text={text} onChange={onChange} />
    </>
  );
};

export default SubA;
```

```javascript
// Sub Component B
const SubB = ({ text, onChange }) => {
  const handleChangeText = (e) => onChange(e.target.value);

  return (
    <div>
      <h2>Sub Component B</h2>
      <div>{text}</div>
      <input type="text" onChange={handleChangeText} />
    </div>
  );
};

export default SubB;
```

`App` 컴포넌트는 `SubA`컴포넌트를 렌더링하고 또 `SubA` 컴포넌트는 하위에 `SubB` 컴포넌트를 받아 렌더링하고 있다. `App` 컴포넌트에서 제공하는 `text` 스테이트와 `setText` 함수는 `SubB` 컴포넌트에서 사용된다. 하지만 prop을 넘겨주는 과정에서 `text`와 `onChange` prop은 `SubA` 컴포넌트를 거쳐가기만 할 뿐이다(Prop Drilling).

```javascript
import { createContext, useState } from "react";
import SubA from "./component/SubA";

export const TextContext = createContext();
export const SetTextContext = createContext();

function App() {
  const [text, setText] = useState("");

  return (
    <TextContext.Provider value={text}>
      <SetTextContext.Provider value={setText}>
        <SubA />
      </SetTextContext.Provider>
    </TextContext.Provider>
  );
}

export default App;
```

- 전역으로 컨텍스트를 생성하고 export 해준다.
- `Provider`로 감싸고 value에 전달하고자 하는 데이터를 넣어준다.
- `Provider`는 중첩될 수 있고 `Provider` 아래에 들어올 하위 컴포넌트는 개수 제한이 없다.

```javascript
import { useContext } from "react";

// export한 컨텍스트를 import 해준다
import { TextContext } from "../App";
import { SetTextContext } from "../App";

const SubB = () => {
  // useContext 훅을 사용해 컨텍스트를 조회해 변수에 담아 사용한다.
  const text = useContext(TextContext);
  const onChange = useContext(SetTextContext);

  const handleChangeText = (e) => onChange(e.target.value);

  return (
    <div>
      <h2>Sub Component B</h2>
      <div>{text}</div>
      <input type="text" onChange={handleChangeText} />
    </div>
  );
};

export default SubB;
```

export한 컨텍스트들을 import해주고 `useContext` 훅을 사용해 컨텍스트를 조회해 변수에 담아 사용한다.

```javascript
import SubB from "./SubB";
// SubA 컴포넌트를 거쳐가기만 하는 prop을 없애준다.
const SubA = () => {
  return (
    <>
      <h2>Sub Component A</h2>
      <SubB />
    </>
  );
};

export default SubA;
```

`context` API를 사용하면 이런식으로 그냥 거쳐가기만 하는 prop을 줄여 좀더 깔끔한 코드를 작성할 수 있다.
