## Read 의 과정

> #### **templates/creat.html** 

``` python
<h1>글 작성하기</h1>
    <form action="" method="POST">
        {% csrf_token %}
        {{ shurdang.as_p }}
        <input type="submit" value="글 작성하기">
    </form>
```



* form 태그

  * 웹 페이지에서의 입력 양식을 의미한다. 데이터를 전달하는 양식!
  * `action` 속성 : action 은 빈칸 이므로 지금의 페이지( 여기서는 url 로 'create/' 를 포함한 페이지 )로 똑같이 이동하게 된다.

* {{ shurdang.as_p }} : p 태그로 shurdang(Board.objects.all) 을 나타낸다.

* submit 버튼을 누르면 'POST' 방식으로 데이터를 전송하게 된다.

* POST 방식이란?

  * ㅇㅇㅇ
  * ㅇㅇㅇ

* ``` python
  from django.urls import path
  from IDCO import views
  
  app_name = "IDCO"
  
  urlpatterns = [
      path('', views.list, name="list"),
      path('create/', views.create, name="create"),
      path('update/<int:pk>/', views.update, name="update"),
  ]
  ```

  urls.py 의 코드이다. 'create/' 의 url 을 받으면 views.create 를 실행하도록 되있다.



> #### **views.py**

``` python
def create(request):
    
    if request.method == "POST":
        print('request.POST', request.POST, type(request.POST))
        board_t = request.POST.get('title')
        board_c = request.POST.get('content')
        print('board_t', board_t, type(board_t))
        print('board_c', board_c, type(board_c))
        form = Board.objects.create(title=board_t, content=board_c)
        print('form', form, type(form))
        return render(request, 'index/success.html')

    else:
    # print('else:', request.method, type(request.method))  # HTML 메서드가 Get 일 경우, 각 매서드와 타입을 출력하여 확인한다.
        youtube = BoardForm()
        # print('youtube', dir(youtube))

    context = { 'shurdang': youtube }
    return render(request, 'index/create.html', context)

```

* 전달방식

  * get
  * ddd
  * post
  * Ddd

* 이때,  **print('request.POST', request.POST, type(request.POST))** 의 출력문을 보면

  > request.POST <QueryDict: {'csrfmiddlewaretoken': ['78ZKizb96XlXIEiEeVyS3IwibuHJ6pBXf2t2utjJwhMBLsbN0GLUJ1ajWxIPhlvb'], 'title': ['dfasdf'], 'content': ['hymyului;']}> <class 'django.http.request.QueryDict'>

  라고 나온다. request.POST 는 QueryDict 의 형태로 정보가 넘어오는 것을 알 수 있다. 

  **print('board_t', board_t, type(board_t))**
  **print('board_c', board_c, type(board_c))**

  > board_t dfasdf <class 'str'>
  > board_c hymyului; <class 'str'>

  이것으로 보아, request.POST.get() 함수를 거쳐 데이터의 형태가 str 로 바뀌었단 것을 알 수 있다.

* **form = Board.objects.create(title=board_t, content=board_c)**

* Board. 은 models.py 에서 선언한 class( 객체 ) 이다



> #### models.py

* 

``` python
from django.db import models


# Create your models here.
class Board(models.Model):

    title = models.CharField(max_length=100)
    content = models.TextField()
```



**print('form', form, type(form))** 를 실행시키면,

> form Board object (28) <class 'IDCO.models.Board'>

가 출력되는 것을 볼 수 있다. form 이란, Board 객체임을 알 수 있고, (28) 은 이 객체가 django의 db 로 부터 정보를 얻어와 출력한 숫자임을 유추할 수 있다. 실제로 models 는 django.db 로 부터 가져온 함수이기 때문에 가능하다.

*  우리는 위 과정을 통해, Board 라는 객체, 즉 models.py 를 거쳐 장고의 데이터베이스와 소통할 수 있다는 것을 알아낼 수 있다. ( 소통: python 에서 squlite 로 번역하여 db 에 전달 )



> #### views.py

* 다시 views.py 로 돌아와 이번에는 아래의 코드로 진행해 보자.

* 

  ```python
  if request.method == "POST":
    print('request.POST', request.POST, type(request.POST))
          form = BoardForm(request.POST)
          print('form', form, type(form))
          # print(form, 'form', type(form))       #   form 은 BoardForm(request.POST) 이다.
          if form.is_valid():
              print(form, type(form))
              form.save()
              print(form, type(form))                         #   
              return render(request, 'index/success.html')
          
  
    else:
    # print('else:', request.method, type(request.method))  # HTML 메서드가 Get 일 경우, 각 매서드와 타입을 출력하여 확인한다.
        youtube = BoardForm()
        # print('youtube', dir(youtube))
  
      context = { 'shurdang': youtube }
      return render(request, 'index/create.html', context)
  ```



* 여기서 BoardForm 은 우리가 forms.py 에서 선언한 class( 객체 ) 이다.

> #### forms.py

* 

  ``` python
  from django import forms
  from .models import Board       # 그 전에 없던 Board 를 import 해와야 한다.
  
  class BoardForm(forms.ModelForm):           # ModelForm 과 Form 이 있는데, Form 은 너무 노가다이다.
      class Meta:
          model = Board
          fields = ['title', 'content'] # '__all__'               #Board 중에 사용할 것을 고른다.
  ```



* BoardForm 안의 class Meta 를  보자.

  * 메타 데이터란?

    데이터의 데이터. 즉, 해당 데이터에 대한 세부 정보이다.

    ex) 디지털 카메라는 카메라의 메타 데이터.

  model = Board 의 식이 있다. 즉, ***BoardForm 역시 models.py 에 있는 객체 Board 와 마찬가지로 django.db 의 models 함수 역할을 할 수 있다는 것이다.*** ( 장고 db 와 소통할 수 있다. )



> #### views.py

* views.py 로 돌아와

  **print('form', form, type(form))** 의 출력문을 확인한다.

  > form <tr><th><label for="id_title">Title:</label></th><td><input type="text" name="title" value="다시다시" maxlength="100" required id="id_title"></td></tr>
  > <tr><th><label for="id_content">Content:</label></th><td><textarea name="content" cols="40" rows="10" required id="id_content">
  > 제대로 출력</textarea></td></tr> <class 'IDCO.forms.BoardForm'>

  form 으로 인해, QueryDict 에서 HTML 로 바뀌어 출력이 된 것을 확인할 수 있다. 

* 이후, form.is_valid() 로 유효성을 판단한 뒤, form.save() 로 데이터를 저장한다. form 은 위에서 설명했듯 장고 db 와 소통 가능하기 때문에 dgango.db 의 내장함수인 .save() 를 통해 저장 역시 가능하다. 때문에 중간중간 print문으로 form 의 타입을 확인해봐도 sqlite 로 변하지 않고 그대로 남아있음을 알 수 있다.

