# 아파치 MPM
아파치 웹 서버에서는 멀티프로세싱 모듈(Multi-Processing Module)로 prefork방식과 worker방식을 지원합니다.
MPM모듈은 요청을 받아들이고, 받아들인 요청을 어떤 방식으로 처리할지 선택할 수 있도록 하여 사이트별 요구조건에 특화된 웹 서버를 구성할 수 있도록 합니다.

저희 조에서는 prefork방식을 사용하고 있었습니다.

## MPM사용하기
아파치를 설치하는 경우 `--with-mpm=worker` 와 같은 옵션을 주어 mpm모듈을 설치할 수 있습니다.

현재 설정된 MPM방식을 보시려면 httpd -V로 확인할 수 있습니다.

## MPM방식의 차이
### prefork 방식
- 프로세스당 스레드를 한개만 사용.
- 하나의 요청에 대해 별개의 프로세스를 사용하기 때문에 안정적(다른 요청에 영향을 주지 않음)
- 자식 프로세스는 최대 1024개까지만 가능
- 요청당 프로세스를 만들다 보니 메모리 사용량이 큼.
### worker 방식
- 하나의 프로세스당 여러개의 worker쓰레드를 사용.
- 쓰레드 방식이기 때문에 메모리 공유, 통신량이 많은 서버에 적절함.
- 멀티 쓰레드에서 발생할 수 있는 race condition 대한 주의가 필요함.

## 사용 가능한 옵션
### prefork 방식
- **StartServer**
	- 아파치가 Start시 생성되는 자식 프로세스의 갯수.
	- MPM 방식마다 default 갯수가 다르다.
- **MinSpareServers**
	- 유휴 프로세스의 최소 갯수.
	- 요청이 많은 사이트에서는 이 수치를 적당히 높게 지정하면 빠른 처리가 가능하다.
	- 이 숫자보다 적은 유휴프로세스가 있을 경우 부모프로세스는 최대 1초에 한개씩 자식 프로세스를 생성한다.
	- Setting this parameter to a large number is almost always a bad idea.
- **MaxSpareServers**
	- MinSpareServers와 비슷하게 유휴 프로세스의 최소 갯수.
	- 유휴 프로세스가 지정된 수치보다 많을 경우 초과된 만큼 프로세스를 kill한다.
	- 만약 MinSpareServers보다 작은 값을 나타낼 경우 아파치가 자동으로 MinSpareServers + 1로 변경한다.
- **MaxClients**
	- 리퀘스트를 처리할 수 있는 자식 프로세스의 최대 갯수.
	- 
- **MaxRequestPerChild**
	- 각각의 자식 프로세스 하나가 요청을 몇번 처리할지 결정. 하나의 자식 프로세스가 지정된 숫자만큼 요청을 처리하고 나면 죽는다.
	- 이 값이 0일 경우 무제한으로 처리한다.
	- KeepAlive 요청은 처음 한번의 요청만 카운팅한다.

- **ListenBackLog**
	- pending 커넥션 큐의 최대 크기를 의미합니다.

### worker 방식
- **StartServers**
	- 아파치가 Start시 생성되는 자식 프로세스의 갯수.
- **ServerLimit**
	-  최대 실행 가능한 프로세스의 갯수
-  **MaxClients**
	- 자식 프로세스의 갯수 * 프로세스 당 쓰레드 갯수
-  **ThreadPerChild**
	-  하나의 자식 프로세스가 만들어질때 생성되는 쓰레드의 갯수
-  **MaxRequestPerChild**
	-  prefork 방식과 같습니다.
-  **MaxSpareThreads**
	-  prefork 방식과 같습니다.
-  **MinSpareThreads**
	-  prefork 방식과 같습니다.

## MaxClients 설정과 관련한 글
http://d2.naver.com/helloworld/132178


## 참고
- http://httpd.apache.org/docs/2.0/ko/mod/mpm_common.html
- http://mindpower.kr/62
