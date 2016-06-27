### csrf(Cross-site request forgery) 
- 사이트간 요청위조/ 웹사이트 취약점 공격의 하나로, 사용자가 자신의 의지와는 상관없이 공격자가 의도한 행위를 특정 웹사이트에 요청하게 하는 공격을 말한다.

- @csrf_exempt 태그를 붙혀준다. 
- django 1.2부터 post로 값을 전송시 CSRF 보안 목적으로 추가된 것이라고 한다.
- POST 전송시 붙히지 않으면 오류발생


##### example

	@csrf_exempt
    def DoWriteBoard(request):
    	br = DjangoBoard (subject = request.POST['subject'],
                      name = request.POST['name'],
                      mail = request.POST['email'],
                      memo = request.POST['memo'],
                      created_date = timezone.now(),
                      hits = 0
                     )
    	br.save()

post형식으로 request를 날릴 때 위에 어노테이션(?)을 붙혀 csrf를 막을 수 있다. 

#### 참고
https://docs.djangoproject.com/en/dev/ref/csrf/

		-끝-
