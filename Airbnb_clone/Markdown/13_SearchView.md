

# 13. SearchView



## 13.0 Template, Form, Url, Setup



## 13.1 Starting the Form



## 13.2 Select Choices



## 13.3 Amenities and Facilities Form



## 13.4 Finishing the Form



## 13.5 Filtering Like a Boss



## 13.6 Filtering Like a Boss Part Tow



## 13.7 Introduction to Django Forms



- 장고는 Forms API를 가지고 있다
  python 파일을 만들면, labels, value, checked 등의 모든 일을 장고가 해준다

- rooms - views.py에 다음 코드 작성

  ```python
  from django.shortcuts import render
  form . import models, forms // forms 추가로 Import
  
  def search(request):
  
  	form = forms.SearchForm()
  	
  	return render(request, "rooms/search.html", {"form" : form})
  ```

- rooms 디렉토리 안에 forms.py 파일 생성 후 아래 코드 작성

  ```python
  from django import forms
  
  class SearchForm(forms.Form)
  
  	pass
  ```

- 연결된 search.html에 다음과 같이 작성 -> {{form}} 추가

  ```django
  {% block content %}
  	<h2> Search! </h2>
  
  	<form method="get" action="{% url "rooms:search" %}">
  		{{form}}
  		<button>Search</button>
  	</form>
  	
  {% endblock content %}
  ```

- Forms도 모델과 비슷하게 fields를 가지고 있다
  Ex) `city = forms.Charfield(initial="Anywhere")`
  `price = forms.IntegerField(required=False)` 

- django documentation에서 Forms API를 확인해보면
  forms를 paragraph, ul, html 등 여러가지 방식으로 나타낼 수 있음
  ex) 위의 {{form}} 부분을 {{form.as_p}} 로 변경

- Room_type을 select하고 싶다면 다음 코드 작성

  ```python
  from . import models // ModelChoiceField는 queryset을 필요로 하므로 모델 import
  
  room_type = forms.ModelChoiceField(queryset=models.RoomType.objects.all())
  ```

  

## 13.8 I love Django Forms ForEver 



- Help_text : 폼 아래 부분에 입력을 도와주는 문구를 넣을 수 있다

  ```python
  guests = models.IntegerField(help_text="How many people will be staying?")
  ```

- form의 field는 기본적으로 html의 widget을 가져오는데 default값을 변경할 수 있다
  ex) charfield는 기본적으로 textfield이지만 textarea로 변경 가능

  ```python
  city = forms.CharField(initial="Anywhere", widget=forms.Textarea)
  ```

  



## 13.9 Forms are Awesome!



-  위의 작업까지 한 경우에서는, 프론트 단에 검색관련 field는 생겼으나, form이 기억하고 있지는 못한 상태

  ```python
  from django.shortcuts import render
  form . import models, forms // forms 추가로 Import
  
  def search(request):
  
  	form = forms.SearchForm(request.GET) // 이부분 추가
  	
  	return render(request, "rooms/search.html", {"form" : form})
  ```

  get을 추가해주면 해결됨

- unbound form : 비어있는 form / form의 시작부분

- bound form : 데이터와 연결되어있는 form
  URL에 무엇이 있는가를 check해서 그 Form을 보여주면 된다

  ```python
  from django.shortcuts import render
  form . import models, forms // forms 추가로 Import
  
  def search(request):
  
  	country = request.Get.get("country")
  
  	if country:
  	
  		form = forms.SearchForm(request.GET)
  		
  		if form.is_valid():
  		
  			city = form.cleaned_data.get("city")
        country = form.cleaned_data.get("country")
        room_type = form.cleaned_data.get("room_type")
  	else:
  		form = forms.SearchForm()
  	
  	return render(request, "rooms/search.html", {"form" : form})
  ```

  


## 13.10 Finishing Up!