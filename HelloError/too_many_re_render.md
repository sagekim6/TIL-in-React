# `Too many re-renders. React limits the number of renders to prevent an infinite loop.`

상황: `useState`로 정의한 `setState`함수를 prop으로 전달해서 `onClick` 이벤트가 생기면 `setState`가 실행되는 코드였다.

```javascript
// 대충 이런 코드였음
function App() {
  const [isClicked, setIsClicked] = useState(false);
  // isClicked를 반대로 바꿔줌
  const handleIsClicked = setIsClicked(!isClicked); // 여기가 문제!
  // useState값을 전부 prop으로 넘겨줌
  return (
    <>
      <Header isClicked={isClicked} handleIsClicked={handleIsClicked} />
      <Main />
    </>
  );
}

export default App;
```

그리고 `<Button />`에 `onClick` 이벤트로 넘겨주었다. 버튼이 클릭되면 isClicked의 값이 바뀔 수 있게.  
state의 값이 바뀌면 컴포넌트는 리렌더링된다. 그리고 렌더링 과정에서 state가 변경되는 함수가 있다면 계속해서 리렌더링이 일어난다.  
왜냐하면 리렌더링되면 state 함수가 재정의 되고 그러면 state가 다시 정의되고 그러면서 다시 렌더링이 일어나면서 너무 많은 리렌더링이 일어난다.

```javascript
const handleIsClicked = () => {
  setIsClicked(!isClicked); // 화살표 함수로 만들어 주면 간단하게 해결된다
};
```

**행결방법은 아주 간단한데 그냥 화살표 함수를 사용해주면 된다**
