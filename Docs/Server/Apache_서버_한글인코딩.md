# 톰캣에서의 URI 인코딩
톰캣의 `server.xml`파일을 확인해보면 기본적으로 두개의 커넥터를 볼 수 있습니다. HTTP 커넥터와 AJP 커넥터인데 저희가 전달받은 `set_webapps.sh`로 설치를 하였다면 HTTP커넥터는 `URIEncoding`이 `UTF-8`로 설정되고 AJP커넥터는 `useBodyEncodingForURI`가 `true`로 활성화되어있습니다. 따로 설정을 수정하지 않았다면 아래의 모습과 유사할 것입니다.

```
<Connector port="9001" protocol="HTTP/1.1"
             connectionTimeout="20000"
             URIEncoding="UTF-8"
             redirectPort="8443" />

 <Connector port="8001" protocol="AJP/1.3"
             enableLookups="false"
             acceptCount="100" debug="0"
             connectionTimeout="10000"
             useBodyEncodingForURI="true"
             maxPostSize="3145728"
             disableUploadTimeout="true"
             redirectPort="8443" />
```
## URIEncoding
커넥터의 `URIEncoding` 속성은 URI를 인코딩하는 방법을 지정하는 것입니다. 저희는 HTTP커넥터가 UTF-8로 설정되어 있습니다. 그렇기 때문에 아파치를 거치지 않고 톰캣으로 바로 웹앱에 접속한다면 URI의 한글 파라미터가 깨지지 않습니다.
하지만, 아파치를 통해 톰캣에 접속하면 톰캣은 HTTP커넥터가 아니라 AJP커넥터를 사용합니다. 그래서 AJP커넥터의 속성을 사용하는데 URIEncoding속성이 설정되어 있지 않고 useBodyEncodingForURI이 true로 설정되어 있습니다.

## useBodyEncodingForURI
`useBodyEncodingForURI`속성을 `true`로 지정하면 톰캣은 URI를 인코딩하는 방식을 웹앱에게 넘깁니다. 웹앱에서 설정한 인코딩 방식으로 URI를 인코딩합니다. 예로들면,

```
request.setCharacterSetEncoding("utf-8");
```
위와 같은 코드를 예로 들 수 있습니다. 또한 Spring에서 지원하는 UTF8 인코딩 필터를 사용할 수도 있습니다. 이처럼 웹앱에서 설정하는 인코딩 방식에 따라 URI를 인코딩합니다.

# 그래서 생긴 문제
## 문제
아파치를 통해 톰캣에 요청을 하는 경우, AJP커넥터를 사용하기 때문에 URI 인코딩 방식을 웹앱에게 맞깁니다. 그런데 웹앱에서도 따로 인코딩 방식을 지정하지 않는다면 톰캣은 Java에서 설정된 기본 인코딩 방식으로 인코딩을 수행하는데, 기본 인코딩이 UTF-8이 아닐 수도 있습니다. 그래서 한글이 우리가 원했던대로 UTF-8로 인코딩되지 않습니다. 

## 해결
이런 상황을 해결하기 위해서 웹앱에서 따로 요청 인코딩을 설정하거나, AJP커넥터의 `useBodyEncodingForURI`속성을 제거하고 `URIEncoding`속성을 사용할 필요가 있습니다.
