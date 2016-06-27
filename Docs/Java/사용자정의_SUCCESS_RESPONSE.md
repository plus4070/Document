## 구현

REST API에서 `DELETE, POST`와 같이 `header만` 클라이언트로 Reponse를 보내기 위해서 아래와 같은 Util 클래스의 static method를 통하여 생성을 해주었습니다.

``` java
public static Map<String, Object> getResultHeaderOnly(boolean isSuccess, int responseCode, String message) {
		Map<String, Object> result = new HashMap<String, Object>();
		JsonHeader header = new JsonHeader(isSuccess, responseCode, message);

		result.put("header", header);

		return result;
	}
```

이때 아래의 예제처럼 SUCESS의 경우는 정해진 포맷을 보내게 됩니다.

ex) 댓글 삭제의 경우
`{resultCode: 200, resultMessage: 댓글이 삭제되었습니다., isSuccessful:true}`

이 코드를 `static 변수`로 미리 선언을 해두면 `new` 를 하며 소비되는 비용을 줄일 수 있습니다.

``` java
public static Map<String, Object> COMMENT_DELETE_SUCCESS = getResultHeaderOnly(true, 200, "삭제 되었습니다.");
```
