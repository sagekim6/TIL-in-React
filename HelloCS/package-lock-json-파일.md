# package-lock.json이란?

## package.json

먼저 package.json 파일을 먼저 알아보자

- package.json이란 현재 프로젝트에 관한 정보와 패키지 매니저(npm, yarn)을 통해 설치한 모듈들의 의존성을 관리하는 파일이다.

&rarr; 즉, **현재 프로젝트를 설명해주는 파일이다.** 사용하는 도구들의 환경설정에 대한 저장소이고 또한 npm과 yarn이 설치된 패키지의 이름과 버저을 저장하는 장소이다.

## package-lock.json

package.json 에서는 특정한 버전을 저장하지 않고 버전의 범위를 저장하기 때문에 package.json 파일을 참고해 `npm install`을 해도 잘 안될 수 있다.  
그 외에도 몇가지 문제로 인해 다른 node_modules 가 생성된다면 package.json이 동일하더라도 개발 테스트가 어떤 컴퓨터에서는 잘 되고, 어떤 컴퓨터에서는 잘 안될 수 있다.(개발 환경이 달라지기 때문)

package-lock.json은 package.json이 수정되거나 node_modules의 구조가 변경될 때 당시의 의존성에 대한 정확하고 구체적인 정보를 가지고 자동 생성이 된다.  
또한 package-lock.json 파일이 존재하는 경우에는 package.json파일을 이용하지 않고 node_modules 파일을 생성한다.

**&rarr; 즉, package-lock.json이라는 파일은 동일한 node_modules를 생성하게 해줘서 같은 의존성을 설치할 수 있게 보장하는 파일이다.**
