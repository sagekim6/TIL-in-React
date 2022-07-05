# React Router

라우터를 사용하기 전에...!

<strong>1. MPA(Multipage Application) 이란?</strong>

- 보통 페이지 라우팅은 웹 서버가 처리하는데 어떤 경로를 가지고 요청이 들어오면  
  <u>여러 개의 페이지 중 해당 경로에 맞는 페이지를</u> 웹 서버가 돌려주는 방식을 MPA(Multipage Application)이라고 한다.

<strong>2. SPA(Single Page Application) 이란? </strong>

- 기존 웹 페이지 처럼(MPA 방식) 여러개의 페이지를 사용, 새로운 페이지를 로드하는 방식이 아닌  
  새로운 페이지를 로드하지 않고 <u>하나의 페이지 안에서</u> 필요한 데이터만 가져오는 형태를 가진다.

리액트는 SPA 프레임워크이고 Router는 흔히 말하는 <b>페이지 이동</b>기능을 가능하게 한다 .

리액트로 만든 사이트를 요청하게 되면 우선 index.html을 받게된다. 그리고 리액트 앱을 통째로 받게 된다.  
이 상태에서 유저가 사이트의 다른 페이지로 이동하고자 할 때는 **서버에 요청을 보내는 것이 아니라 리액트가 알아서 페이지를 업데이트 시키게 된다.**  
이렇게 <u>클라이언트 측에서 알아서 페이지를 렌더링하는 것을 CSR(client side rendering)</u>이라고 한다.

## `Router` 시작하기

제일 먼저 터미널에 명령어로 라우터를 다운받아준다.

```javascript
npm i react-router-dom
```

아래는 `App` 컴포넌트에 `Home` 컴포넌트와 `New` 컴포넌트를 렌더링한 코드이다.

```javascript
const Home = () => {
  return <h2>This is Home.</h2>;
};

export default Home;
```

```javascript
const New = () => {
  return <h2>This is New.</h2>;
};

export default New;
```

```javascript
function App() {
  return (
    <>
      <h2>App.js</h2>
      <Home />
      <New />
    </>
  );
}

export default App;
```

### 1. `<BrowserRouter>` 사용

- `App` 컴포넌트에 `<BrowserRouter>`를 임포트 해준다.
- BrowserRouter 컴포넌트는 react-router-dom 내장 컴포넌트로 HTML5의 History API를 사용하여 페이지 새로고침 없이 주소변경, props 내려주기를 할 수 있도록 한다.

```javascript
// BrowserRouter 임포트
import { BrowserRouter } from "react-router-dom";

function App() {
  // 제일 상단위치에서 다른 컴포넌트를 감싸준다.
  return (
    <BrowserRouter>
      <h2>App.js</h2>
      <Home />
      <New />
    </BrowserRouter>
  );
}

export default App;
```

### 2. `<Routes>`, `<Route>` 사용

- `<Routes>`, `<Route>`를 임포트 해준다.
- `<BrowserRouter>`아래에 `<Routes>`, `<Route>` 컴포넌트를 사용한다.
- `<Routes>`컴포넌트는 여러 `Route`를 감싸서 그 중 규칙이 일치하는 라우트 단 하나만을 렌더링 시켜주는 역할을 한다.
- `<Route>` 컴포넌트는 path 속성과 element 속성을 갖는다.
  - path: 경로를 지정해준다.
  - element: 렌더링하고자 하는 컴포넌트를 넣어준다.

```javascript
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./Pages/Home";
import Edit from "./Pages/Edit";

function App() {
  return (
    <BrowserRouter>
      <h2 className="App">App js</h2>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/new" element={<New />} />
      </Routes>
      <RoutTest />
    </BrowserRouter>
  );
}

export default App;
```

### 3. `<Link>` 사용

보통 html에서 링크를 사용할 때는 `<a>`태그의 href속성을 사용해서 링크를 걸었지만 리액트 라우트는 `<Link>` 컴포넌트를 사용한다.

- 먼저 `<Link>` 임포트 하기
- `to` 속성을 이용해 경로를 지정한다. `<a>` 태그의 href 속성과 비슷하다.

```javascript
// Link 컴포넌트 임포트
import { Link } from "react-router-dom";

const RoutTest = () => {
  return (
    <>
      <Link to={"/"}>Home</Link>
      <br />
      <Link to={"/new"}>New</Link>
    </>
  );
};

export default RoutTest;
```

방금 작성한 RoutTest 컴포넌트를 App 컴포넌트에 렌더해주면 화면에 링크가 잘 보이게 된다.

```javascript
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./Pages/Home";
import New from "./Pages/New";
import RoutTest from "./components/RouteTest";

function App() {
  return (
    <BrowserRouter>
      <h2 className="App">App js</h2>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/new" element={<New />} />
      </Routes>
      <RoutTest />
    </BrowserRouter>
  );
}

export default App;
```

해당 링크를 클릭하면 깜빡이는 현상없이 페이지 이동이 잘 되는 것을 볼 수 있다.
