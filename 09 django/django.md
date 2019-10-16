# django-tut

django

1. django web frame work суулгах
  ```pip install django```
  
2. django application project үүсгэх
  ```django-admin startproject projectname```
  
3. web application ажиллуулах
  ```python manage.py runserver```
  **ажиллуулахдаа терминал project дотор байхыг анхаарах**
  
4. Үндсэн прожектод байрлах түүний нэртэй адилхан хавтсанд views.py файл үүсгэж дараах кодыг шивнэ.

```
from django.http import HttpResponse

def about(request):
    return HttpResponse('about') 
```
    
5. urls.py файлд web app-н uri хаягууд ямар хүсэлт авж ямар функц ажиллуулахыг зааж өгдөг.
```
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('about/', views.about)
]
```
Дээрх кодыг шивээд ажиллуулаад http://127.0.0.1:8000/about/ хандаж үзнэ.


6. Web application үүсгэх ```python manage.py startapp blog```
7. blog хавтсанд urls.py файл үүсгэж дараах кодыг шивнэ
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.posts),
]
```
8. blog хавтас дахь views.py файлд дараах кодыг шивнэ.
```
from django.shortcuts import render

def posts(request):
    return render(request, 'posts.html')
```
9. Үндсэн app -н settings.py файлын дараах хэсэгийн кодонд 'blog' app-аа нэмж өгнө. //доорх код адил
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog'
]
```

мөн дараах код адил өөрчлөлт хийнэ.
```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
10. blog хавтас дахь models.py файлд модел үүсгэнэ.
```
class Post(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()
    date = models.DateTimeField(auto_now_add=True)
```
11. database migrate хийх ```python manage.py migrate``` командаар хийнэ.
12. model migrate хийх ```python manage.py makemigrations```.
13. ```python manage.py migrate``` дахин migrate хийнэ.
14. shell programm ашиглан өгөгдөл үүсгэх 
```
python manage.py shell
from blog.models import Post
Post
Post.objects.all()
p.title = 'Hello'
p.body = 'We are python programers'
p.title
p.save()
Post.objects.all()
Post.objects.all()[0].title
```
15. Web application admin хэсэгт нэвтрэхдээ эхлээд супер хэрэглэгч үүсгэнэ.
```python manage.py createsuperuser```
http://127.0.0.1:8000/admin/ хаягаар орно.
16. blog хавтасан дахь admin.py файлд үүсгэсэн моделоо нэмж өгснөөр админ хэсгээс удирдах боломжтой болдог.
```
from django.contrib import admin
from .models import Post
admin.site.register(Post)
```
17. model-д дараах кодыг нэмнэ.
```
def __str__(self):
        return self.title
```

18. blog app -н views.py файлыг дараах байдалтай болго
```
from django.shortcuts import render
from .models import Post

def posts(request):
    posts = Post.objects.all().order_by('date')
    return render(request, 'posts.html', {'data': posts})
```
19. blog app-н templates-н posts.html файлыг дараах байдалтай болго
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Blog Posts</title>
</head>
<body>
    <h1>Blog Posts</h1>
    <div class="posts">
        {% for i in data %}
        <h2><a href="#">{{ i.title }}</a></h2>
        <p>{{ i.body }}</p>
        <p>{{ i.date }}</p>
        {% endfor %}
    </div>
</body>
</html>
```
19. ажиллуулаад http://127.0.0.1:8000/posts/ орох
20. Хэд хэдэн пост нэмж оруулах урт байвал зүгээр
21. blog app -н модел болон posts.html файлд дараах өөрчлөлтийг оруулна.
**posts.html**
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Blog Posts</title>
</head>
<body>
    <h1>Blog Posts</h1>
    <div class="posts">
        {% for i in data %}
        <h2><a href="#">{{ i.title }}</a></h2>
        <p>{{ i.snippet }}</p>
        <p>{{ i.date }}</p>
        {% endfor %}
    </div>
</body>
</html>
```

**models.py**
```
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()
    date = models.DateTimeField(auto_now_add=True)
    
    def __str__(self):
        return self.title

    def snippet(self):
        return self.body[:50] + '...'
```
22. static файл нэмэх 
үндсэн аппын settings.py файлд дараах кодыг нэмнэ
```
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'assets'),
)
```
23. үндсэн аппын urls.py файлд дараах кодыг шивнэ
```
from django.contrib import admin
from django.urls import path, include
from django.contrib.staticfiles.urls import staticfiles_urlpatterns
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.home),
    path('about/', views.about),
    path('posts/', include('blog.urls'))
]

urlpatterns += staticfiles_urlpatterns()
```
24. glob app -н posts.html template-н head хэсэгт дараах кодыг нэмнэ
```
<link rel="stylesheet" href="/static/style.css">
```
25. үндсэн хавтасд assets полдер үүсгэн style.css файл үүсгэж дараах кодыг шивээд http://127.0.0.1:8000/posts/ хандана
```
body { background: burlywood }
```

26. template extend хийх: үндсэн хавтасд templates хавтас үүсгэн layout.html үүсгэж дараах кодыг шивнэ
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="/static/style.css">
    <title>Blog Posts</title>
</head>
<body>
    <div class="wrapper">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```
27. blog app -н posts.html template-н кодыг дараах кодоор сольж ажиллуулна.
```
{% extends 'layout.html' %}


{% block content %}
<h1>Blog Posts</h1>
<div class="posts">
    {% for i in data %}
    <h2><a href="#">{{ i.title }}</a></h2>
    <p>{{ i.snippet }}</p>
    <p>{{ i.date }}</p>
    {% endfor %}
</div>
{% endblock %}
```
28. blog app-н posts.html файлын кодыг дараах кодоор өөрчлөнө.
```
{% extends 'layout.html' %}


{% block content %}
<h1>Blog Posts</h1>
<div class="posts">
    {% for i in data %}
    <h2><a href="{% url 'post' id=i.id %}">{{ i.title }}</a></h2>
    <p>{{ i.snippet }}</p>
    <p>{{ i.date }}</p>
    {% endfor %}
</div>
{% endblock %}
```
29. blog app-н urls.py файлын кодыг дараах кодоор сольж өөрчлөөд ажиллуулна.
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.posts, name="posts"),
    path('<int:id>', views.post, name="post")
]
```

30. blog app-н views.py файлын кодыг дараах кодоор сольж өөрчилнө.
```
from django.shortcuts import render
from django.http import HttpResponse
from .models import Post

def posts(request):
    posts = Post.objects.all().order_by('date')
    return render(request, 'posts.html', {'data': posts})


def post(request, id):
    post = Post.objects.get(id = id)
    
    return render(request, 'post.html', {'data': post})
    # return HttpResponse(id)
 ```
 31. blog app -н templates дотор post.html файл үүсгэж дараах кодыг хуулаад ажиллуулна.
 ```
 {% extends 'layout.html' %}

{% block content %}

<div class="post">
    <h2>{{ data.title }}</h2>
    <p>{{ data.body }}</p>
    <p>{{ data.date }}</p>
</div>


{% endblock %}
 ```
32. үндсэн app-н urls.py файлын кодыг дараах кодоор солино.
```
from django.contrib import admin
from django.urls import path, include
from django.contrib.staticfiles.urls import staticfiles_urlpatterns
from django.conf.urls.static import static
from django.conf import settings
from . import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.home),
    path('about/', views.about),
    path('posts/', include('blog.urls'))
]

urlpatterns += staticfiles_urlpatterns()
urlpatterns +=static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)
```
33. settings.py файлын төгсгөлд дараах кодыг нэмнэ
```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR,'media')
```
34. blog app-н models.py файлын Post классд дараах талбарыг нэмнэ
```photo = models.ImageField(default='default.png',blank=True)```
35. blog app-н post.html template-д дараах кодыг нэмнэ
```<img src="{{data.photo.url}}" alt="zurag"/>```
36. дараах командуудыг биелүүлнэ
```pip install pillow
  python manage.py makemigrations
  python manage.py migrate
  python manage.py runserver
  ```
37. admin хэсгээс ямар нэгэн постыг зургийг өөрчилж үзнэ
38. Хэрэглэгч удирдах user app үүсгэх
```python manage.py startapp user```
39. үндсэн app-н settings.py installed_app хэсэгт нэмэх
40. үндсэн app-н urls.py файлд дараах кодыг нэмнэ
```path('',include('user.urls'))```
41. user app-н views.py файлд дараах кодыг нэмнэ
```from django.shortcuts import render

# Create your views here.
def signup_view(request):
    return render(request, 'signup.html')
```
42. user app-н urls.py файлд дараах кодыг нэмнэ
```
from django.urls import path
from .import views

urlpatterns = [
    path('signup', views.signup_view, name="signup"),
]
```
43. user app-д templates хавтас үүсгэх signup.html файл үүсгэж дараах кодыг шивнэ
```
{% extends 'layout.html' %}
{% block content %}
    <h1>Sign up</h1>
{% endblock %}
```
  http://127.0.0.1:8000/signup

44.user app-н views 
```
from django.shortcuts import render, redirect
from django.contrib.auth.forms import UserCreationForm

# Create your views here.
def signup_view(request):
    print(request)
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('posts')
    else:
        form = UserCreationForm()
    return render(request, 'signup.html', {'form': form})
```
45. user app-н signup.html 
```
{% extends 'layout.html' %}

{% block content %}

    <h1>Sign up</h1>
    <form action="/signup" method="POST">
        {% csrf_token %}
        {{ form }}
        <input type="submit" value="Sign up">
    </form>

{% endblock %}
```
46. user app-д login.html үүсгэн дараах кодыг бичнэ.
```
{% extends 'layout.html' %}

{% block content %}

    <h1>Login</h1>
    <form action="{% url 'login' %}" method="POST">
        {% csrf_token %}
        {{ form }}
        <input type="submit" value="Login">
    </form>

{% endblock %}
```
47. user app-н urls.py-д дараах кодыг нэмнэ ```path('login', views.login_view, name="login")```
48. user app-н views.py-д дараах хэлбэртэй болго
```
from django.shortcuts import render, redirect
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth import login
# Create your views here.
def signup_view(request):
    print(request)
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request,user)
            return redirect('posts')
    else:
        form = UserCreationForm()
    return render(request, 'signup.html', {'form': form})


def login_view(request):
    if request.method == 'POST':
        form = AuthenticationForm(data=request.POST)
        if form.is_valid():
            user = form.get_user()
            login(request,user)
            return redirect('posts')
    else:
        form = AuthenticationForm()
    return render(request, 'login.html', {'form': form})
```
49. layout дараах кодыг нэмнэ
```
<form action="{% url 'logout' %}" method="POST">
        {% csrf_token %}
        <button type="submit">Logout</button>
    </form>
```
50. user app-н views.py-д дараах кодыг нэмнэ
```
def logout_view(request):
    if request.method == 'POST':
        logout(request)
        return redirect('home')
```
51. user app-н urls.py-д дараах кодыг нэмнэ
``` path('logout', views.logout_view, name="logout"),```

52. blog app-н views.py-д дараах кодыг нэмнэ
```
from django.contrib.auth.decorators import login_required
@login_required(login_url="login")
def create_post(request):
    return render(request, 'create-post.html')
```
53. blog app-н  urls.py-д
```path('create',views.create_post, name='create_post')```
54. blog app-д create-post.html үүсгэнэ
```
{% extends 'layout.html' %}
{% block content %}
<h1>Create post</h1>
<div class="post">
</div>
{% endblock %}
```
55. blog app-д forms.py файл үүсгэнэ
```
from django import forms
from .models import Post
class CreatePost(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'body', 'image']
```
56. create-post.html
```
{% extends 'layout.html' %}
{% block content %}
<h1>Create post</h1>
<form action="{% url 'create_post' %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{form}}
    <button type="submit">Save</button>
</form>
{% endblock %}
```
57. PostController
```
def create_post(request):
    if request.method == 'POST':
        form = CreatePost(request.POST, request.FILES)
        if form.is_valid():
            #save to db
            return redirect('posts')
    else:
        form = CreatePost()
        return render(request, 'create-post.html', {'form': form})
```
58. models.py Post model-д дараах талбарыг нэмнэ.
```
author = models.ForeignKey(User, default=None, on_delete=models.DO_NOTHING)
```
59. Бүх мэдээллээ устгаад migration хийнэ
60. PostController.py
```
def create_post(request):
    if request.method == 'POST':
        form = CreatePost(request.POST, request.FILES)
        if form.is_valid():
            # save to db
            post = form.save(commit=False)
            post.author = request.user
            post.save()
            return redirect('posts')
    else:
        form = CreatePost()
        return render(request, 'create-post.html', {'form': form})
```
61. 
