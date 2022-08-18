# 리액트에서 `svg` 사용하기

이번에 frontend-mentor 챌린지를 하면서 처음으로 svg 이미지를 사용해 보았다.  
생각보다 까다롭다고 느껴졌지만 검색해보니 여러가지 장점이 많은 것 같아 사용법을 정리해본다.

## 1. `<img />` 태그 사용

svg 아이콘을 임포트해와서 src 속성값으로 넣어준다. width와 height값만 변경가능하다.

```javascript
import ArrowDown from "../images/icon-arrow-down.svg";

const Icon = () => {
  return (
    <div>
      <img src={ArrowDown} alt="ArrowDown" />
    </div>;
  )
};
```

## 2. 컴포넌트 사용

ReactComponent로 임포트해주고 `as [컴포넌트명]`으로 컴포넌트 이름을 지정해준다.

```javascript
import { ReactComponent as ArrowDown } from "../images/icon-arrow-down.svg";
```

지정해준 컴포넌트 이름으로 svg를 컴포넌트로 사용가능하다.

```javascript
import { ReactComponent as ArrowDown } from "../images/icon-arrow-down.svg";

// 컴포넌트로 사용가능
const Icon = () => {
  return <ArrowDown />;
};
```

svg 파일에서 변경하고 싶은 부분을 **`current`으로 지정**하고 **prop으로 넘겨주는 식으로 변경**가능하다.

```javascript
<svg
  width="current" // width="10" -> width="current" 으로 변경
  height="6"
  viewBox="0 0 10 6"
  xmlns="http://www.w3.org/2000/svg"
>
  <path stroke="#686868" stroke-width="1.5" fill="none" d="m1 1 4 4 4-4" />
</svg>
```

```javascript
import { ReactComponent as ArrowDown } from "../images/icon-arrow-down.svg";

// current으로 지정해둔 부분을 prop으로 넘겨주면서 값을 변경시킨다.
const Icon = () => {
  return <ArrowDown width={20} />;
};
```
