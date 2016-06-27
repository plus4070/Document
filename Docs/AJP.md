# Apache HTTP 서버와 Tomcat 서버의 연동

## Apache, Tomcat 연동의 이유
Tomcat은 WAS 서버이지만 Web 서버의 기능도 갖추고 있는 WAS 서버입니다.

그러나 톰캣의 Web 서버 기능은 아파치보다 느린 속도처리를 보였고, 이로 인해 정적인 페이지는 Apache가 처리하고, 동적인 페이지를 Tomcat이 처리함으로써 부하를 분산하는 이유에서 Apache와 Tomcat을 연동하였습니다.

그러나 이는 옛날 얘기이고.. 지금은 Tomcat이 많이 발전해 Tomcat 내의 Web 서버가 아파치에 절대 뒤쳐지지 않을만큼의 역할을 수행합니다.

그럼에도 불구하고 아직도 Apache와 Tomcat을 연동하여 사용하는 이유는, 아파치 내에서만 설정할 수 있는 부분이라던가 아파치에서 제공하는 유용한 모듈들을 톰캣에서 사용할 수 없기 때문입니다.

## AJP
아파치와 톰캣이 연동하기 위해선 AJP를 통해 서로 통신을 하여야 합니다.

AJP란 아파치가 웹서버와 외부 서비스(톰캣 등)과 연동하기 위해 정한 규약(프로토콜) 입니다. 현재 1.3까지 나와있습니다.

아파치는 이를 사용하여 80포트로 들어오는 요청은 자신이 받고, 이 요청중 서블릿을 필요로 하는 요청은 톰캣에 접속하여 처리합니다.

우리는 아파치 톰캣 연동을 위해 mod_jk 라는 모듈을 사용할건데, 이는 AJP 프로토콜을 사용하여 톰캣과 연동하기 위해 만들어진 모듈입니다.

mod_jk는 톰캣의 일부로 배포되지만, 아파치 웹서버에 설치하여야 합니다.

## 처리플로우
1. 아파치 웹서버의 httpd.conf 에 톰캣 연동을 위한 설정을 추가하고 톰캣에서 처리할 요청을 지정함.

2. 사용자의 브라우저는 아파치 웹서버(보통 80포트)에 접속해 요청

3. 아파치 웹서버는 사용자의 요청이 톰캣에서 처리하도록 지정된 요청인지 확인. 요청을 톰캣에서 처리해야 하는 경우 아파치 웹서버는 톰캣의 AJP포트(보통 8009포트)에 접속해 요청을 전달

4. 톰캣은 아파치 웹서버로부터 요청을 받아 처리한 후, 처리 결과를 아파치 웹서버에 되돌려줌

5. 아파치 웹서버는 톰캣으로부터 받은 처리 결과를 사용자에게 전송

## 설정파일 수정
~ 톰캣이 하나일 경우
- workers.properties


    worker.list=worker1

    worker.worker1.type=ajp13

    worker.worker1.host=localhost

    worker.worker1.port=8009
    
    
~ 톰캣이 여러개의 경우
- workers.properties


    worker.list=worker1,worker2 // 이름은 임의로 설정

    worker.worker1.type=ajp13

    worker.worker1.host=localhost

    worker.worker1.port=8009

    #worker.worker1.lbfactor=1


    worker.worker2.type=ajp13

    worker.worker2.host=localhost

    worker.worker2.port=8010 // 포트만 바꿔주시면 됩니다. 사용할 두번째 톰캣의 server.xml 파일을 수정하셔야 됩니다.(위에서 보여드린 부분)

    #worker.worker2.lbfactor=2

`여기서 주석처리되었지만 추가된 부분이 있는데, worker.worker명.lbfactor 라는 구문입니다. 이는 작업할당량 비율이라고 보시면됩니다. worker1:worker2 = 1:2 의 비율로 작업을 처리한다고 보시면 됩니다.`
