---
layout: post
title:  "파이썬 웹 프로그래밍 공부 실전편 5"
date:   2022-09-10 22:50:13 +0900
categories: python_web_programming_practice
---


장고와 파이썬의 버전이 안 맞아서 오류가 났었다. 파이썬은 3.8.9 버전을 쓰고 있는데, 장고는 최신 버전을 깔려고 해서 호환이 안 되었던 것 같다. 호환을 확인하는 방법은 pypi.org에서 장고를 검색해 보면 된다. 다음과 같은 정보를 확인할 수 있다.
<pre>
Meta
License: BSD License (BSD-3-Clause)

Author: Django Software Foundation

Requires: Python >=3.8
</pre>

파이썬 3.8도 가능한 것처럼 표시되긴 했는데... 어쨌든 장고를 낮은 버전으로 깔았더니 문제가 해결되었다.



## 4장 프로젝트 첫 페이지 만들기

base.html

{% raw %}
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta http-equiv=X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>{% block title %}Django Web Programming{% endblock %}</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
</head>

  {% block extra-style %}{% endblock %}

<body style="padding-top:90px;">

<nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">
  <div class="container-fluid">
    <span class="navbar-brand mx-5 mb-0 font-weight-bol font-italic">Django - Python Web Programming</span>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item mx-1 btn btn-primary">
          <a class="nav-link text-white" aria-current="page" href="{% url 'home' %}">Home</a>
        </li>
        <li class="nav-item mx-1 btn btn-primary">
          <a class="nav-link text-white" href="{% url 'bookmark:index' %}">Bookmark</a>
        </li>
        <li class="nav-item mx-1 btn btn-primary">
          <a class="nav-link text-white" href="{% url 'blog:index' %}">Blog</a>
        </li>
        <li class="nav-item mx-1 btn btn-primary">
          <a class="nav-link text-white" href="#">Photo</a>
        </li>
        <li class="nav-item dropdown mx-1 btn btn-primary">
          <a class="nav-link dropdown-toggle text-white" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown">Util</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'admin:index' %}">Admin</a></li>
            <li><a class="dropdown-item" href="{% url 'blog:post_archive' %}">Archive</a></li>
            <li><a class="dropdown-item" href="#">Search</a></li>
          </ul>
        </li>
      </ul>
      <form class="form-inline my-2" action="" method="post">
        {% csrf_token %}
        <input class="form-control mr-sm-2" type="search" placeholder="global search" name="search_word">
      </form>
    </div>
  </div>
</nav>

<div class="container">
  {% block content %}{% endblock %}
</div>


{% block footer %}{% endblock %}

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.10.2/dist/umd/popper.min.js" integrity="sha384-7+zCNj/IqJ95wo16oMtfsKbZ9ccEh31eOz1HGyDuCQ6wgnyJNSYdrPa03rtR1zdB" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.min.js" integrity="sha384-QJHtvGhmr9XOIpI6YVutG+2QOK9T+ZnN4kzFN1RtK3zEFEIsxhlmWl5/YESvpZ13" crossorigin="anonymous"></script>

<script src="https://kit.fontawesome.com/36c998c735.js" crossorigin="anonymous"></script>

{% block extra-script %}{% endblock %}
</body>
</html>
```
{% endraw %}


home.html

{% raw %}
```html
{% extends 'base.html' %}

{% load static %}

{% block title %}home.html{% endblock %}

{% block extra-style %}
<style type="text/css">
.home-image{
  background-image: url("{% static 'img/swimming-pool.jpg' %}");
  background-repeat: no-repeat;
  background-position: center;
  background-size: 100%;
  height: 500px;
  border-top: 10px solid #ccc;
  border-bottom: 10px solid #ccc;
  padding: 20px 0 0 20px;
}
.title {
  color: #c80;
  font-weight: bold;
}
.powered {
  position: relative;
  top: 77%;
  color: #cc0;
  font-style: italic;
}
</style>
{% endblock extra-style %}


{% block content %}

<div class="home-image">
  <h2 class="title">Django - Python Web Programming</h2>
  <h4 class="powered"><i class="fas fa-arrow-circle-right"></i>powered by django and bootstrap.</h4>
</div>

<hr style="margin: 10px 0;">

<div class="row text-center">
  <div class="col-sm-6">
    <h3>Bookmark App</h3>
    <p>Bookmark is a Uniform Resource Identifier (URI)
      that is stored for later retrieval in any of various storage formats.
      You can store your own bookmarks by Bookmark application.
      It's also possible to update or delete your bookmarks.</p>
  </div>

  <div class="col-sm-6">
    <h3>Blog App</h3>
    <p>This application makes it possible to log daily events or write your own interests
    such as hobbies, techniques, etc.
    A typical blog combines text, digital images, and links to other blogs, web pages, and other media related to its topic.</p>
  </div>
</div>
{% endblock content %}


{% block footer %}
<footer class="fixed-bottom bg-info">
  <div align="right" class="text-white font-italic mr-5">Copyright &copy; 2022 Django site by moonsunmi</div>
</footer>
{% endblock footer %}
```
{% endraw %}


## 5장 개선하기

base.html 템플릿을 상속 받아서 사용하게 만든다.


post_all.html
{% raw %}
```html
{% extends 'base.html' %}

{% block title %}post_all.html{% endblock %}

{% block content %}

{% for post in posts %}
    <h3><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h3>
    {{ post.modify_dt|date:"N d, Y" }}
    <p>{{ post.description }}</p>
{% endfor %}

<br>


<div>
    <span>
    {% if page_obj.has_previous %}
      <a href="?page={{ page_obj.previous_page_number }}">PreviousPage</a>
    {% endif %}

    Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}

    {% if page_obj.has_next %}
      <a href="?page={{ page_obj.next_page_number }}">NextPage</a>
    {% endif %}
  </span>
</div>

{% endblock %}
```
{% endraw %}


## 6장 - Tag 달기

가장 많이 사용하는 django-taggit 패키지를 선택.
django-taggit-templatetags2 패키지도 사용.


models.py
{% raw %}
```python
from django.db import models
from django.urls import reverse
from taggit.managers import TaggableManager

# Create your models here.


class Post(models.Model):
    title = models.CharField(verbose_name='TITLE', max_length=50)
    slug = models.SlugField(verbose_name='SLUG', unique=True, allow_unicode=True, help_text='one word for title alias.')
    description = models.CharField(verbose_name='DESCRIPTION', max_length=100, blank=True, help_text='simple description text')
    content = models.TextField(verbose_name='CONTENT')
    create_dt = models.DateTimeField(verbose_name='CREATE DATE', auto_now_add=True)
    modify_dt = models.DateTimeField(verbose_name='MODIFY DATE', auto_now=True)
    tags = TaggableManager(blank=True)


    class Meta:
        verbose_name = 'post'
        verbose_name_plural = 'posts'
        db_table = 'blog_posts'
        ordering = ('-modify_dt',)

    def __str__(self):
        return self.title

    def get_absolute_url(self):
        return reverse('blog:post_detail', args=(self.slug,))

    def get_previous(self):
        return self.get_previous_by_modify_dt()

    def get_next(self):
        return self.get_next_by_modify_dt()



```
{% endraw %}

admin.py
{% raw %}
```python
from django.contrib import admin
from blog.models import Post
# Register your models here.

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'modify_dt', 'tag_list')
    list_filter = ('modify_dt',)
    search_fields = ('title', 'content')
    prepopulated_fields = {'slug': ('title',)}

    def get_queryset(self, request):
        return super().get_queryset(request).prefetch_related('tags')

    def tag_list(self, obj):
        return u", ".join(o.name for o in obj.tags.all())
```
{% endraw %}


N:N 관계에서 쿼리 횟수를 줄여 성능을 높이고자 할 때는 prefetch_related() 메서드를 사용한다.


post_detail.html
{% raw %}
```html
{% extends 'base.html' %}

{% block title %}post_detail.html{% endblock %}

{% block content %}
<h2>{{ object.title }}</h2>
<p>
  {% if object.get_previous %}
    <a href="{{ object.get_previous.get_absolute_url }}" title="View previous post">
      <i class="fas fa-arrow-circle-left"></i> {{ object.get_previous }}
    </a>
  {% endif %}

    {% if object.get_next %}
      | <a href="{{ object.get_next.get_absolute_url }}" title="View next post">
        {{ object.get_next }} <i class="fas fa-arrow-circle-right"></i>
      </a>
    {% endif %}
</p>

<p>{{ object.modify_dt|date:"j F Y" }}</p>
<br>

<div>
  {{ object.content|linebreaks }}
</div>

<br>

<div>
  <b>TAGS</b><i class="fa-solid fa-tag"></i>
  {% load taggit_templatetags2_tags %}
  {% get_tags_for_object object as "tags" %}
  {% for tag in tags %}
  <a href="{% url 'blog:tagged_object_list' tag.name %}">{{ tag.name }}</a>
  {% endfor %}
  &emsp;
  <a href="{% url 'blog:tag_cloud' %}"><span class="btn btn-info btn-sm">TagCloud</span></a>
</div>


{% endblock %}
```
{% endraw %}



taggit_cloud.html
{% raw %}
```html
{% extends 'base.html' %}

{% block title %}taggit_cloud.html{% endblock %}

{% block extra-style %}
<style type="text/css">
.tag-cloud {
  width: 40%;
  margin-left: 30px;
  text-align: center;
  padding: 5px;
  border: 1px solid orange;
  background-color: #ffc;
}
.tag-1 { font-size: 12px; }
.tag-2 { font-size: 14px; }
.tag-3 { font-size: 16px; }
.tag-4 { font-size: 18px; }
.tag-5 { font-size: 20px; }
.tag-6 { font-size: 24px; }
</style>

{% endblock extra-style %}

{% block content %}
  <h1>Blog Tag Cloud</h1>
  <br>
  <div class="tag-cloud">
    {% load taggit_templatetags2-tags %}
    {% get_tagcloud as tags %}
    {% for tag in tags %}
    <span class="tag-{{tag.weight|floatformat:0}}">
      <a href="{% url 'blog:tagged_object_list' tag.name %}">{{ tag.name }}({{tag.num_times}})</a>
    </span>
    {% endfor %}
  </div>

{% endblock %}

```
{% endraw %}

{{tag.weight|floatformat:0}}: tag.weight는 실수형인데, floatformat:0 템플릿 필터에 의해 반올림하면 정수형으로 변환된다.



taggit_post_list.html

{% raw %}
```html
{% extends 'base.html' %}

{% block title %}taggit_post_list.html{% endblock %}

{% block content %}
<h1>Post for tag - {{ tagname }}</h1>
<br>

{% for post in object_list %}
    <h3><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h3>
    {{ post.modify_dt|date:"N d, Y" }}
    <p>{{ post.description }}</p>
{% endfor %}

{% endblock content %}


```
{% endraw %}






