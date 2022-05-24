# ```useEffect(callback, [dependencies])```
- 첫 번째 콜백에는 실행시키고 싶은 코드를 넣고  
- 두번째 인자에는 배열안에 검사하고자하는 특정값 혹은 빈 배열을 넘겨준다.  
- 대표적으로 api 호출, 타이머 등 랜더링 과정에서 한 번만 호출해도 될 기능이 매번 실행될 때 사용  
- 해당 컴포넌트의 랜더링 완료 후에 실행  
  -> ```useMemo()```는 랜더링 중에 실행되는 차이가 있다.  
### 1. import 해주기  
- ```useEffect```를 사용하기 위해선 제일 먼저 import를 해줘야한다.

```javascript
import { useEffect } from "react";
```
### 2. deps 자리에 빈 배열을 넣게 되면  
- 검사하고자 하는 특정값이 비어있기 때문에 한번만 랜더링되고 더이상 랜더링 되지 않는다.  
```javascript
useEffect(() => {
  console.log('Rendered!') // 'Rendered!' -> 콘솔에 한번만 나타남
}, [])
```
- 배열자체를 생략하면 랜더링될 때마다 실행된다.
```javascript
useEffect(() => {
  console.log('Rendered!') // 'Rendered!' -> 랜더링될 때마다 콘솔에 찍힌다.
}) // 두번째 인자 생략
```
### cleanUp Function

```javascript
import { useEffect, useState } from "react";

function Hello() {
  // 컴포넌트가 처음 실행될 때만 동작한다
  useEffect(() => {
    console.log("hi!");
    return () => {
      console.log("bye!");
    };
  }, []);

  return <h1>Hello</h1>;
}

function App() {
  const [showing, setShowing] = useState(false);

  const changeShowing = () => {
    setShowing((prev) => !prev);
  };

  // show버튼을 누르면 Hello 컴포넌트가 생성됬다가 Hide 버튼을 누르면 Hello 컴포넌트가 삭제된다.
  // 그리고 다시 show 버튼을 누르면 Hello 컴포넌트가 실행되는데 이때 useEffect도 다시 실행된다.
  return (
    <>
      {showing ? <Hello /> : null}
      <button onClick={changeShowing}>{showing ? "Hide" : "Show"}</button>
    </>
  );
}

export default App;
```
