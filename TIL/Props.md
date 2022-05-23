# Props

- 리액트에서 컴포넌트에 속성을 지정할 수 있는데 이렇게 지정한 속성을 ```Props```라고 한다.
- ```Props```는 Properties의 약자로 지정된 속성이 <b>하나의 객체로</b> 모여서 컴포넌트를 정의한 첫 번째 파라미터로 전달된다.  

```javascript

// 컴포넌트에 text prop을 넘겨준다
function Button({ text }) {
  return (
    <>
      <button>{text}</button>
    </>
  );
}

function App() {
  return (
    <>
      <h1>Hello World</h1>
      <Button text="Click me!" /> // text 속성 추가
    </>
  );
}

export default App;
```

## children

- Props에는 ```{children}```이라는 특별한 프로퍼티가 있다.
- 컴포넌트를 단일 태그가 아니라 <b>여는 태그와 닫는 태그의 형태로 작성</b>하면 그 안에 담기는 내용은 ```children```값에 담기게 된다.
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
      <Button>Bye~</Button> // 그 안에 내용(텍스트)작성 -> 컴포넌트의 children 인자로 전달
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
      <MemoBtn text="bye~" /> // 3. 하지만 래핑한 MemoBtn을 컴포넌트로 사용시 -> 해당 버튼은 리랜더링되지 않는다
    </>
  );
}

export default App;
```
## ```React.PropTypes```
- props의 타입을 검사하여 잘못된 타입의 prop이 입력되는지 체크한다.  
- ```prop-types```에서 ```PropTypes```을 import하여 사용  

``` javascript
import PropTypes from "prop-types"; // 1. PropTypes를 import

function Button({ text, fontSize }) {
  return (
    <>
      <button style={{ fontSize }}>{text}</button>
    </>
  );
}

// Button 컴포넌트의 proptype을 지정해준다
Button.propTypes = {
  text: PropTypes.string, 
  fontSize: PropTypes.number,
};

function App() {
  return (
    <>
      <h1>Hello World</h1>
      <!-- text prop은 문자열을 받아야하지만 숫자를 받고 있다. -> Warning: Failed prop type 콘솔에 에러 띄워준다 -->
      <Button text={1} fontSize={23} />
    </>
  );
}

export default App;
```
