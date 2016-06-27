# 아파치 서버와 톰캣의 연결 부분

## 환경설정
설치를 마무리하셨다면 `/home1/irteam/apps` 경로에 아파치 서버 및 톰캣이 설치되었을 것입니다. 이미 아파치 서버와 톰캣 서버가 연동되어 있기 때문에 환경설정만 해주면 됩니다.

### 아파치 서버 설정
irteamsu로 로그인 후 아파치 서버로 이동
```
$ cd /home1/irteam/apps/apache
```
httpd 설정 파일을 엽니다.
```
$ vim ./conf/httpd.conf
```
아래로 쭉 내려오거나 검색을 해보면 다음의 내용을 보실 수 있습니다.
```
<IfModule mod_jk.c>
....
	<Location /jkmanager/>
    ...
    </Location>
...
</IfModule>
```

`<IfModule mod_jk.c>` 태그 안에 보면 `JkMount`라는 것이 있는데 여기 작성된 주소를 바탕으로 아파치 서버에서 톰캣 서버로 요청을 전달할 수 있습니다. 만일 자신의 조의 주소가 `000.000.000.000/abc`라면 
```
<IfModule mod_jk.c>
JkMount /abc/* tomcat
....
	<Location /jkmanager/>
    ...
    </Location>
...
</IfModule>
```
위와 같이 설정한다면 아파치 서버로 들어오는 요청의 주소 경로가 /abc로 시작한다면 톰캣에서 처리를 합니다.

### 톰캣 서버 설정
톰캣 디렉토리의 `conf`폴더로 가면 `server.xml`이라는 XML파일이 있습니다. 이 파일을 열고 아래 부분을 보면 `<Connector>`태그가 있는데 기본적으로 HTTP 커넥터와 AJP 커넥터가 있습니다. HTTP 커넥터를 확인하시면 포트와 같은 여러 설정을 볼 수 있습니다. AJP 커넥터는 아파치 서버와의 연결에 사용되는 커넥터인데, 현재 이미 설정이 되어 있기 때문에 건드릴 필요는 없습니다. 그 아래의 `<Engine>`태그를 보면 내부에 `<Context>`태그가 있는데 이 태그를 통해 자신의 웹 애플리케이션과 경로를 설정할 수 있습니다. 예를 들어,
```
<Context path="/page" docBase="page"/>
```
위와 같이 되어 있다면, `/page`경로로 들어오는 요청은 `page`라는 웹앱에서 담당하겠다는 뜻입니니다. 웹앱은 `<Host>`태그의 `appBase`에서 설정한 경로에서 찾습니다(기본값은 webapps입니다). 웹앱인 만큼 war파일도 가능합니다.
```
<Context path="" docBase="webFile.war"/>
```
