# `Encountered two children with the same key, '1'.`

- `map` 함수를 사용할 때는 고유한 key값을 같이 넘겨주어야 하는데 동일한 key값을 가지게 되면 발생하는 에러이다.
- <strong>문제파악을 먼저 해준다</strong>: 리액트 개발자 도구를 사용해 어디서 key값이 겹치는지 확인해준다.

### + `map`함수 사용시...

- 중괄호{}를 사용한다면 `return`을 빼먹지 말자
- 고유한 `key`값을 사용해주자.
- `map`은 배열 내장 함수이므로 배열에 사용해야한다.
