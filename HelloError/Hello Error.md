## `Uncaught TypeError: destroy is not a function`

코드

```javascript
// 영화 정보를 불러오는 함수
const getMovies = async () => {
  const response = await fetch(
    `https://yts.mx/api/v2/list_movies.json?minimum_rating=8.8&sort_by=year`
  );
  const result = await response.json();

  setMovies(result.data.movies);
  setLoading(false);
};

useEffect(() => getMovies(), []); // 여기가 문제!
```

- 원인 : `useEffect`의 기능 중에 cleanUp function이 있다. `useEffect`함수가 뭔가를 리턴한다면 무조건 cleanUp function이 된다.  
   내가 원하는 것은 `getMovies()`함수가 리턴되는 것이 아니라 실행되는 건데 위의 코드는 중괄호{}를 생략하면서 return 키워드가 생략된 형태의 코드가 되었다.

```javascript
useEffect(() => return getMovies(), []); // 이런 형태의 코드가 된다.
```

간단하게 중괄호{}만 붙여주면 해결된다.

```javascript
useEffect(() => {
  getMovies();
}, []); // 문제 해결!
```
