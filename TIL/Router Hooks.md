# React Router Hooks

## 1. `useParams`

- `<Route path>`속성의 경로와 매치되는 현재 url을 객체로 리턴한다.
- URL에 **변수**를 담아서 어떤 값을 전달하는 방법을 <u>Path Variable</u>이라고 한다.  
  이때 이 변수를 `useParams` 리액트 라우터 훅을 이용해 받아와 사용할 수 있다.

```javascript
import * as React from 'react';
import { Routes, Route, useParams } from 'react-router-dom'; // 1. useParams 임포트 해주기

function ProfilePage() {
  // path 속성의 userID를 받아온다
  let { userId } = useParams();
  // ...
}

function App() {
  // :userID는 동적으로 변하는 값(변수)이되고 useParams가 리턴하는 객체의 key값이 된다.
  return (
    <Routes>
      <Route path="users">
        <Route path=":userId" element={<ProfilePage />} />
        <Route path="me" element={...} />
      </Route>
    </Routes>
  );
}
```

## 2. `useSearchParams`

- **Query String(쿼리 문자열)** : name과 value를 엮어서 데이터를 전송하는 기법. 웹 페이지에 데이터를 전달하는 가장 간단한 방법이다.  
  &rarr; ex) `/edit?id=10&mode=dark`에서 물음표 뒤에 `id=10`,`mode=dark` 부분이 쿼리 문자열이된다.
- Path variable을 사용할 때는 `<Route>`의 path속성에 변수를 추가해주어야 했지만 쿼리 문자열을 사용하면 따로 변수가 없어도 자동으로 페이지 이동이 된다.  
  &rarr; 즉, 쿼리 문자열은 페이지 이동에 영향을 주지 않는다고 보면 된다.
- `useSearchParams`는 현재 url 위치의 **쿼리 문자열을 읽고 수정할 수 있다.**
- 리액트의 `useState` 훅처럼 배열의 형태로 요소 두개를 리턴하는데
  - `0`: 현재 위치의 쿼리
  - `1`: 쿼리를 수정할 수 있는 setter 함수를 리턴한다.

```javascript
let [searchParams, setSearchParams] = useSearchParams();
```

`searchParams`의 `get`메소드는 쿼리 문자열을 조회할 수 있고 `setSearchParams` 메소드는 쿼리를 업데이트 할 수 있다.

```javascript
import { useSearchParams } from "react-router-dom"; // 1. useSearchParams를 import 해준다

const Edit = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  // /edit?id=10&mode=dark <- 이런 쿼리 문자열이 있다고 할 때
  const id = searchParams.get("id"); // get 메소드를 사용해 id를 조회함
  const mode = searchParams.get("mode");
  console.log(id); // 10
  console.log(mode); // dark

  return <h2>This is Edit.</h2>;
};

export default Edit;
```

## 3. `useNavigate`

페이지를 이동시키는 함수를 반환한다. 그 함수를 변수에 담아 사용하는데 인자로 path값을 넣어주면 해당 경로로 이동한다.

```javascript
import { useNavigate } from "react-router-dom"; // 1. useNavigate 임포트 해주기

const Edit = () => {
  const navigate = useNavigate(); // 반환된 함수를 변수에 담아서 사용

  return (
    <>
      <h2>This is Edit.</h2>
      <button onClick={() => navigate("/")}>Go Home</button>
    </>
  );
};

export default Edit;
```
