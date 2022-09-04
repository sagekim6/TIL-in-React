# REST api

## API(Application Programing Interface)

- 한 프로그램에서 다른 프로그램으로 **데이터를 주고받기 위한 방법**
- 식당과 손님이 **메뉴판을 통해 주문**하고 서로 의사소통한다. 여기서 **메뉴판의 역할이 API라고 보면된다.**

### API의 역할

**1. 서버와 데이터베이스의 출입구같은 역할을 한다.**  
 : API를 사용해 허용된 사람들에게만 서버와 데이터베이스에 접근할 수 있도록 한다.  
**2. 애플리케이션과 기기가 원활하게 통신할 수 있도록 한다.**  
 : API는 애플리케이션과 기기가 데이터를 원활히 주고받을 수 있도록 돕는 역할을 합니다.  
**3. 모든 접속을 표준화한다.**  
 : API는 모든 접속을 표준화하기 때문에 누구나 동일하게 접근 가능하다.

## REST api (Representational State Transfer api)

- REST 방식으로 설계된 API를 REST api라고 한다.

### `REST` (Representational State Transfer)

- `REST`에서는 데이터를 '자원' 즉, Resource라고 부르며 이 **Resource를 이름으로 구분하고 해당 Resource의 상태를 주고받는것** 을 말한다.

예를 들어 아래와같은 데이터(Resource)가 있다면

**topic**
| id | title | body |
| :-: | :---: | :--: |
| 1 | rest | ... |
| 2 | ajax | ... |
| 3 | json | ... |

- **Collection** : **topic 전체**를 식별하거나 **여러개의 데이터**를 식별하는 것을 말한다.
  - `http://example.com/topics` &rarr; **복수형으로 작성한다.**
- **Element** : **데이터를 하나 하나**를 Element 라고 한다.

  - `http://example.com/topics/1`또는 `http://example.com/topics/rest`로 **id나 단수형태로 표현**되고
  - Element가 모여서 Collection이 되고 Collection의 데이터 하나 하나를 element라고 한다.

**&rarr; `http://example.com/topics/1` 이런 방식은 URI로 단순히 resource를 식별하는 이름을 적은 것 뿐이고 이 resource를 가공하는 방법에는 CRUD가 있다.**

- REST는 http 방식을 이용하기 때문에 http의 메소드와 특징을 이용할 수 있다.

1. Create(POST) - 데이터 생성
2. Read(GET) - 데이터 조회
3. Update(PUT/PATCH) - 데이터 수정
4. Delete(DELETE) - 데이터 삭제

### &rarr; 다시 말해 REST란

- URI로 자원를 식별하고
- http 메소드를 이용해 해당 자원에 CRUD 작업을 하는 것을 말한다.

**그리고 이러한 규칙에 따라 설계된 API를 REST api라고 하고 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있다.**
