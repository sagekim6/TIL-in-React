# `TypeError`와 `somthing is not a function`모음

## 1. `Uncaught TypeError: destroy is not a function`

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

## 2. `TypeError: articles.map is not a function`

```javascript
// 영화 한편의 자세한 정보를 가져오는 함수
function Details() {
  const [loading, setLoading] = useState(true);
  const [details, setDetails] = useState([]);

  const { id } = useParams();
  const getDetails = async () => {
    const res = await fetch(`some kind of url with ${id}...`);
    const json = await res.json();
    setDetails(json.data.movie); // 여기가 문제!
    setLoading(false);
  };
}

useEffect(() => {
  getDetails();
}, []);
```

위 상황에서 `details.map()`을 하니 생긴 에러. 일단 <u>`map()`은 배열에만 사용할 수 있다</u>는 건 알고 있었는데 뭐가 문제 인지 잘 몰랐다.  
알고 보니 내가 useState를 잘못 이해하고 있었다. 결국 `setDetails(json.data.movie)`안에 들어가는 저 데이터는 배열이 아니라 객체였고 각괄호[]로 감싸주니 해결되었다.

```javascript
setDetails([json.data.movie]);
```

나는 useState에 초기값으로 빈 배열을 넘겨주었으니 `setDetails`로 바꾼 값은 초기값으로 넘겨준 빈 배열에 담길거라 생각했고 그래서 당연히 `details`는 배열이라고 생각했는데 아니었다.
