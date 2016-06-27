## Model.py를 이용한 DB Table생성
- django는 ORM(Object Relational Mapping) 기능을 자체적으로 제공하고 있다. 
- 모델을 이용해서 실제 데이터베이스 테이블과 맵피되서 동작되고, SQL을 직접 작성하지 않고도 데이터의 조작을 가능하게 해준다.
- 직접 SQL을 설정하여(Raw SQL) 데이터를 조작할 수 있다.

        class DjangoBoard(models.Model):
            id = 0
            subject = ''
            name = ''
            created_date = ''
            mail = 0
            memo = ''
            hits = 0
           
을 Table로 만들 예정

### examlpe ###
	class DjangoBoard(models.Model):
    	subject = models.Charfield(max_length=50, blank=True)
        name = models.CharField(max_length=50, blank=True)
        created_date = models.DateField(null=True, blank=True)
        mail = models.CharField(max_length=50, blank=True)
        memo = models.CharField(max_length=200, blank=True)
        hits = models.IntegerFiled(null=True, blank=True)
        Category = models.ForeignKey(Categories)
- 일련번호(id)는 django 에서 자동으로 만들어서 관리한다. 새로운 자료가 들어오면 알아서 가장 마지막 id 값에 1을 더해서 id 를 만든다. 그래서 어차피 id 라는 이름을 가진 일련번호 요소를 쓸 것이라면 굳이 정의할 필요가 없다.

- 외래키를 선언할 때는 models.ForeignKey를 이용하여 생성하는데 Categoies class는 DjangoBoard위에 선언되어야 한다.

자세한 내용은 http://blog.hannal.com/2008/6/04_2-python_django_lecture/ 참조
