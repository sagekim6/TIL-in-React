# JSX 문법

자바스크립트의 확장 문법이고 자바스크립트와 html을 섞어서 쓸 수 있다.  
리액트로 개발할 때 JSX 문법이 필수는 아니다. 하지만 장점이 많기 때문에 권장되는 부분이다.  
  
1. 간결한 코드  
```javascript
// jsx 사용 안함
React.createElement('div', null, `Hello, ${name}`); 
```
```javascript
// jsx 사용
<div>Hello, {name}</div>
```
둘은 동일한 기능을 하지만 jsx가 훨씬 간결하다.  
  
2. 가독성 향상. 코드의 의미를 빠르게 파악할 수 있다. &rarr; 유지보수하기 쉽다.  
  
3. Injection Attacks 방어하여 보안성을 높인다.  
입력창에 문자나 숫자같은 일반적인 입력대신 소스 코드를 입력해 해당 코드가 실행되도록 하는 해킹방법이다.  
리액트는 삽입된 값을 렌더링 하기 전에 문자열로 변환한다. 그렇기 때문에 명시적으로 선언되지 않은 값은 중괄호{}에 들어갈 수 없다.  
이것은 XSS라 불리는 해킹을 방지한다.  

## 1. html과 다른 속성명

- 여러 단어로 조합한 속성명은 카멜케이스로 작성  
  `onclick` -> `onClick`  
  `onblur` -> `onBlur`  
  `onFocus` -> `onFocus` ...  
  하지만 비표준 속성을 다룰 때 사용하는 data- 속성은 html문법 그대로 작성한다.

## 2. 자바스크립트 예약어와 같은 속성명은 사용불가

- HTML의 for의 경우에는 자바스크립트의 반복문 키워드 for와 겹치기 때문에 `htmlFor`로,  
  HTML의 class 속성도 자바스크립트의 클래스 키워드 class와 겹치기 때문에 `className`으로 작성한다.

```javascript
function App() {
  return;
  <div>
    // JSX에선 htmlFor로 작성한다
    <label htmlFor="name">What is your name?</label>
    <input id="name" placeholder="write your name" />
  </div>;
}
```

## 3. Fragment

- <u>반드시 하나의 태그로 감싸진 태그를 작성해야한다.</u> 이때 불필요한 태그가 추가될 수 있는데  
  `<Fragment>` 태그를 감싼뒤 추가해주면 `<Fragment>`태그는 없어지고 자식 태그들만 추가된다.
- 그냥 빈 태그`<>`로 축약해서 사용가능

## 4. 자바스크립트 표현식 넣기

- 중괄호{}를 활용하면 자바스크립트 표현식을 넣을 수 있다.
- 속성값에도 따옴표 대신 중괄호를 사용한다.
