## PowerMock

유닛 테스트는 모든 함수에 대해 테스트 케이스를 짜는 것인데 private 함수는 호출이 되지 않아 난감하였습니다.

  
검색을 해보았는데, 리플렉션을 이용할 수도 있었으나 powerMock라는 좋은 라이브러리가 있어서 자료 공유합니다.
  
https://github.com/jayway/powermock
여기는 powermock github 주소입니다.
  
http://mvnrepository.com/artifact/org.powermock/powermock-module-junit4
여기에 보시면 depedency 정보가 버전에 따라 있습니다.
  
```
<dependency>
	<groupId>org.powermock</groupId>
	<artifactId>powermock-module-junit4</artifactId>
	<version>1.6.3</version>
</dependency>

```
저는 1.6.3버전을 사용하였고, Junit 버전에 따라 영향을 받으니 참고 바랍니다.
  
pom.xml에 dependency를 추가하고, 사용방법은 간단합니다.
  
```
WhiteboxImpl.invokeMethod(호출 객체, "메소드 이름", parameter);
```
  

저는 boolean을 리턴하는 함수에서, 아래와 같이 사용하였습니다.
```
boolean isValidUser = WhiteboxImpl.invokeMethod(questionService, "isValidUser", testUserId);
assertThat(isValidUser, equalTo(true));
```
  
private 함수 테스트하려고 public으로 변경하지 않고 진행하시면 됩니다.
