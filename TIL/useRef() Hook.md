# useRef() Hook

DOM요소의 접근에 접근하기 위해 사용되는 React Hook이다. DOM을 직접 선택할 때 사용한다.  

1. `useRef` 훅을 import 해준다
```javascript
import { useRef } from "react";
```

2. `useRef` 함수를 호출해서 반환값을 변수에 저장한다.
  - `useRef` 함수는 `React.MutableRefObject`를 반환하는데 html DOM요소에 접근할 수 있도록 한다.  

```javascript
function DiaryEditor(){
  const autherInput = useRef(); 
  const contentInput = useRef();
  
  // ...다른 코드
  
}
```

3. `ref` prop으로 변수를 넣어준다.

```javascript
const DiaryEditor = () => {
  const autherInput = useRef();
  const contentInput = useRef();

  return (
      <div>
        <input
          ref={autherInput} <-- ref prop으로 useRef() 함수의 반환값이 담긴 변수를 할당한다 -->
          name="auther"
          value={state.auther}
          onChange={handleChangeState}
        />
        <textarea
          ref={contentInput}
          name="content"
          value={state.content}
          onChange={handleChangeState}
        />
    </div>
  );
};
```
## `current` 프로퍼티

`useRef` 함수는 `current` 프로퍼티를 가지는데 이 속성은 우리가 선택하고자 하는 DOM 요소를 가리킨다.

```javascript
const DiaryEditor = () => {
  const autherInput = useRef();
  const contentInput = useRef();
  
  const [state, setState] = useState({
    auther: "",
    content: "",
  });

  const handleChangeState = (e) => {
    setState({
      ...state,
      [e.target.name]: e.target.value,
    });
  };

  const handleSubmit = () => {
    if (state.auther.length < 1) {
      autherInput.current.focus(); // 여기서 current는 input을 가리킨다
      return;
    }

    if (state.content.length < 5) {
      contentInput.current.focus(); // 여기서 curretn는 textarea를 가리킨다
      return;
    }
    alert("저장되었습니다.");
  };

  return (
    <div className="DiaryEditor">
      <h2>오늘의 일기</h2>
      <div className="InputField">
        <input
          ref={autherInput}
          name="auther"
          value={state.auther}
          onChange={handleChangeState}
        />
        <textarea
          ref={contentInput}
          name="content"
          value={state.content}
          onChange={handleChangeState}
        />
      </div>
      <button onClick={handleSubmit}>저장</button>
    </div>
  );
};
```
