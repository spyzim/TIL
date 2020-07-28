# ReMyDjango



django 를 하기 전에 django를 사용할 수 있는 환경을 만들어 준다. 

케이크 구조 -> pyenv 위에 virtualenv 위에 django



> pyenv install '파이썬 버젼'
>
> 	- 사용할 파이썬의 버젼을 설치
>
> pyenv virtualenv '파이썬 버젼' '프로젝트 이름'
>
> 	- 가상환경 설정
>
> pyenv activate '프로젝트 이름'
>
> 	- 가상환경 활성화
>
> pip list
>
> Pip install django(==2.2.13)
>
> 	- 가상환경 위에 django 설치

프로젝트를 실행할 폴더를 만들어 그곳으로 이동한다.

>django-admin startproject config .
>
> * 프로젝트 시작
> * 폴더 안에 config, manage.py 이 생성된다.
>
>python manage.py runserver
>
>	* 로켓 확인.





### 앱

1. 앱 생성하기

> python manage.py startapp '앱이름'



2. 앱 등록하기
   * setting.py 에 들어가 프로젝트에 대한 설정(INSTALLED_APPS) 에 '앱이름'을 등록한다.



### 모델 생성

* MVT 구조
  * M(모델): 데이터베이스나 파일 등의 데이터 소스를 제어, 데이터를 표현하는데 사용되며, python 클래스의 형식으로 정의된다. 하나의 모델 클래스는 데이터베이스에서 하나의 테이블로 표현. django 의 
  * V(뷰): HTTP Request를 받아 HTTP Response를 리턴하는 컴포넌트, 모델로부터 데이터를 읽거나 저장
  * T(템플릿):  사용자에게 보여지는 부분
* 모델은 python 클래스이며, django.db.models.Model 클래스를 상속받는 서브 클래스



1. models.py 파일에 클래스 객체를 생성하여 만든다.

   ~~~ python
   class Boards(models.Model):
     title = models.CharField(max_length=100)
     content = models.TextField()
   ~~~

   



### Migrations



> python manage.py showmigrations
>
> python manage.py makemigrations
>
> python manage.py migrate

* Migration 을 하지 않은 상태에서 runserver 를 하면, 빨간색 에러문이 뜬다.
* Migration 이란 모델에서 설정한 데이터베이스의 테이블 구조를 데이터베이스에 적용시키기 위한 기록이다.
* 앱들이 필요로하는 **데이터베이스 구조를 기록**한 것이 Migration 파일, 이를 **데이터베이스에 적용**시키는 것이 migrate 이다.

1. > python manage.py migrate

migrate 를 하고나면 db.squlite3 라는 파일이 생긴다.



### admin

1. 모델을 관리자 페이지를 통해 사용하기 위해 admin.py 에 등록한다.

~~~ python 
admin.site.register(Boards)
~~~

2. > python manage.py createsuperuser

   사용자명과 이메일, 비밀번호를 설정한다.

3. runserver 하여 설정한 사용자명과 비밀번호로 접속한다.

* models.py 

  ~~~ python
  def __str__(self):
    return self.title
  ~~~





### View

사용자의 request 에 따라 사용자가 접속할 수 있도록 하자.

views.py 를 통해 뷰를 관리한다.

1. views.py 를 열어 뷰를 작성한다

   ~~~ python
   def helloworld(request):
     return HttpResponse('hello world!')
   ~~~

   