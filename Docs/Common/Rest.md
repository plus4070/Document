# REST(REpresentational Safe Transfer) API #

## REST API? ##

`REST(REpresentational Safe Transfer) API`는 웹의 창시자 중 한사람인 Roy Fielding의 논문에 소개된 네트워크 기반의 아키텍쳐 모델이다. 웹의 장점을 최대한 활용하기 위해서 적용된 모델로 3가지 요소로 구성된다.

<br>

### REST의 요소 ###

REST는 리소스, 메소드, 메세지의 3가지 요소로 구성된다.

- 리소스 = 통신으로 생성되는 결과물
- 메소드 = 통신을 하는 메소드(GET, POST, DELETE 등이 해당된다)
- 메세지 = 메소드의 내용

이 기본 요소를 사용하여 유저를 생성하는 메소드를 REST 형태를 만들어보자.

```
HTTP POST, http://webpage.com/user
{
	"user":{
    	"name":"Junyoung"
    }
}
```
유저를 생성하기 때문에 `CRUD`에서 Create에 해당되며 이는 POST 메소드를 사용하여 구현된다.
