# 사이드 이펙트(Side Effect)와 React Hook

## 사이드 이펙트(Side Effect)

함수 안에서 함수 바깥에 있는 값이나 상태를 변경하는 하거나  
개발자가 원하는 결과 이외에 다른 외부의 부수적인 작용이 생기는 것을 <b>사이드 이펙트(Side Effect)</b>라고 한다.

```javascript
let count = 0;

function add(a, b) {
  const result = a + b;
  count += 1; // 함수 내부에서 외부의 값을 변경한다 => 전역으로 선언된 count 변수의 값이 변함
  return result; // 이런 함수를 사이드 이펙트가 있다고 한다.
}

const val1 = add(1, 2);
const val2 = add(-4, 5);
```

`console.log`도 값을 계산해서 리턴하는게 아니라 콘솔창에 문자열을 출력하므로 사이드 이펙트가 있다고 할 수 있다.

## `useEffect`

- `useEffect`는 리액트 컴포넌트 안에서 <u>사이드 이펙트를 처리할 때 사용</u>하는 함수이다.
- DOM노드를 변경, 리퀘스트 보내기 등등 주로 리액트 외부에 있는 데이터나 상태를 변경할 때 주로 사용한다.
- <b>동기화</b>에 사용하면 유용한 경우가 많다.  
  &rarr; <b>동기화란...?</b> 컴포넌트 안에 데이터와 리액트 바깥에 있는 데이터를 일치시키는 것

```javascript
const INITAL_TITLE = "Untitled";

// 핸들러를 사용한 변경
function App() {
  const [title, setTitle] = useState(INITAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
    document.title = nextTitle; // 외부의 상태를 변경하므로 사이드이펙트에 해당한다.
  };

  const handleClearClick = () => {
    const nextTItle = INITAL_TITLE;
    setTitle(nextTItle);
    document.title = nextTitle; // 중복되는 코드 발생
  };

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}
```

위의 코드는 `title`스테이트를 변경한 후에 `document.title`도 같이 변경하고 있다.  
위의 코드를 `useEffect`를 사용해 사이드 이펙트를 처리하면 중복되는 코드를 줄이고 동작을 쉽게 예측할 수 있는 코드를 작성할 수 있다.

```javascript
const INITAL_TITLE = "Untitled";

function App() {
  const [title, setTitle] = useState(INITAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
  };

  const handleClearClick = () => {
    const nextTItle = INITAL_TITLE;
    setTitle(nextTItle);
  };

  // useEffect 사용
  // title 스테이트가 변경될 때마다 콜백실행
  useEffect(() => {
    document.title = title;
  }, [title]);

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}
```
