# `CORS error`

## 동일 출처 정책(Same Origin Policy, SOP)

- **동일 출처 정책(SOP)이란...?**  
  동일 출처 정책(SOP)은 Same Origin Policy의 약자로 같은 오리진(출처)끼리만 데이터를 송수신 할 수있는 정책이다.  
  여기서 **오리진(Origin)이란 프로토콜과 호스트를 합친 것**을 말한다. &rarr; **즉, 동일한 출처간에만 요청, 응답을 허용한다.**  
  <img src="https://ko.javascript.info/article/url/url-object.svg" alt="url" />
- 브라우저는 내 서버가 아닌 곳에서 받아온 데이터는 차다한다. **사용자가 방문하는 사이트를 신뢰하지 않기 때문**이다.  
  왜냐하면 브라우저에는 토큰이나 쿠키에 사용자의 정보가 저장되어 있는데 해커가 이를 탈취해 출처가 다른 사이트에 실어 보내면 심각한 문제가 되기 때문이다.  
  &rarr; 그래서 이를 막기위해 생겨난 것이 **동일 출처 정책**이다.

## 교차 출처 리소스 공유(Cross Origin Resource Sharing, CORS)

동일 출처 정책 때문에 **다른 출처로 송수신을 하게 되면 나는 에러가 `CORS error`다.**

- 교차 출처 리소스 공유는 서로 다른 오리진(교차 출처)에서도 요청과 응답을 혀용하는 정책이다.

### 단순 요청(Simple Request)

### 프리플라이트(Preflight)

### Credentialed Request
