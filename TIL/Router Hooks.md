# React Router Hooks

## 1. `useParams`

- `<Route path>`속성의 경로와 매치되는 현재 url을 객체로 리턴한다.

```javascript
import * as React from 'react';
import { Routes, Route, useParams } from 'react-router-dom'; // 1. useParams 임포트 해주기

function ProfilePage() {
  // path 속성의 userID를 받아온다
  let { userId } = useParams();
  // ...
}

function App() {
  // :userID는 동적으로 변하는 값이되고 useParams이 리턴하는 객체의 key값이 된다.
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

- 웹 페이지에 데이터를 전달하는 가장 간단한 방법이다.
- 현재 url 위치의 쿼리 문자열을 읽고 수정할 수 있다.
- 리액트의 `useState` 훅처럼 배열의 형태로 요소 두개를 리턴하는데
  - `0`: 현재 위치의 쿼리
  - `1`: 쿼리를 수정할 수 있는 setter 함수를 리턴한다.

```javascript
let [searchParams, setSearchParams] = useSearchParams();
```

## 3. `useNavigate`
