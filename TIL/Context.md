### Prop Drilling 문제가 발생

리액트로 props를 넘겨주다 보면 오로지 하위 컴포넌트로 전달하기만 하는 props가 존재한다.  
해당 props는 컴포넌트 트리의 **한 부분에서 다른 컴포넌트로 이동하기 위해서만** 쓰이는데 이것을 <u>Prop Drilling</u>이라고 한다.

&rarr; 이 문제를 해결할 수 있는 방법이 바로 `Context` API를 사용하는 것이다.

# `Context` API

context를 이용하면 단계마다 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있다.  
보통 리액트는 단방향 흐름으로 데이터를 전달하는데 context를 이용하면 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있다.

모든 데이터를 가지고 있는 컴포넌트가 `<Provider />`라는 자식 컴포넌트에게 자신이 가지고 있는 모든 데이터를 전부 준다.  
이 `<Provider />` 컴포넌트는 자신의 자손 컴포넌트에 직통으로 데이터를 내려줄 수 있다. 그리고 `<Provider />` 컴포넌트가 공급하는 모든 데이터에 접근할 수 있는 하위 컴포넌트들을 문맥(context)이라고 표현한다.

### 1. `Context` 생성

```javascript
const MyContext = React.createContext(defaultValue);
```

### 2. Context Provider를 통한 데이터 공급

```javascript
<MyContext.Provider value={전역으로 전달하고자 하는 값}>
  // 하위 컴포넌트가 위치할 곳
</MyContext.Provider>
```
