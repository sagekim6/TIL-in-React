# `Props`의 기본값과 타입지정

상위 컴포넌트에서 `props`를 받아올 때 값이 없거나 혹은 잘못된 타입의 값이 넘오는 것을 방지하기 위해 기본값과 타입을 초기에 지정해준다.

## default Props 설정

### 1. `component.defaultProps` 사용

```javascript
import Item from "./Item";

// lists prop을 받아온다.
const List = ({ lists }) => {
  return (
    <>
      <h3>{lists.length} 개의 일기가 있습니다.</h3>
      <ul className="List">
        {lists.map((list) => {
          return <Item key={list.id} {...list} />;
        })}
      </ul>
    </>
  );
};

// 받아온 prop의 초기값을 빈 배열로 지정한다
List.defaultProps = {
  Lists: [],
};

export default List;
```

### 2. 구조 분해 할당의 기본값 사용

`component.defaultProps`보다 간결하게 작성할 수 있다.

```javascript
import Item from "./Item";

// 인자로 받오는 부분에서 바로 기본값을 지정한다.
const List = ({ lists = [] }) => {
  return (
    <>
      <h3>{lists.length} 개의 일기가 있습니다.</h3>
      <ul className="List">
        {lists.map((list) => {
          return <Item key={list.id} {...list} />;
        })}
      </ul>
    </>
  );
};

export default List;
```

## `Props`의 타입 설정

### `propTypes` 사용

PropTypes는 부모로부터 전달받은 prop의 데이터 type을 검사한다.  
자식 컴포넌트에서 명시해 놓은 데이터 타입과 부모로부터 넘겨받은 데이터 타입이 일치하지 않으면 콘솔에 에러 경고문이 띄워진다.

1. 임포트 해준다.

```javascript
import PropTypes from "prop-types";
```

2. 타입 지정

```javascript
component.propTypes = {
  name: PropTypes.string,
};
```

```javascript
import Item from "./Item";

const List = ({ lists = [] }) => {
  return (
    <>
      <h3>{lists.length} 개의 일기가 있습니다.</h3>
      <ul className="List">
        {lists.map((list) => {
          return <Item key={list.id} {...list} />;
        })}
      </ul>
    </>
  );
};

// 받아온 lists prop의 타입이 배열임을 명시한다. 만약 다른 타입의 값이 들어오면 콘솔에 경고 문구가 출력된다.
List.propTypes = {
  lists: PropTypes.array,
};

export default List;
```
