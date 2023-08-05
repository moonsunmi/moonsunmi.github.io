---
layout: post
title: "파이썬 웹 프로그래밍 공부 실전편 4"
date: 2022-09-09 22:38:33 +0900
tags: python-web-programming-practice
---

## 북마크 앱 만들기

{% raw %}

```python
@admin.register(BookMark)
class BookmarkAdmin(admin.ModelAdmin):
    list_display = ('id', 'title', 'url')
```

{% endraw %}

python3 manage.py showmigrations: 모든 마이그레이션을 보여주고, 각 마이그레이션별 적용 여부를 알 수 있다.

python3 manage.py sqlmigrate bookmark 0001: bookmark 앱의 0001번 마이그레이션을 적용할 때 사용될 SQL 문장을 보여준다.

장고가 알아서 해 주는 것?
{% raw %}

```python
class BookList(ListView):
    model = Book

```

{% endraw %}

Book 테이블로부터 모든 레코드를 가져와 object_list라는 컨텍스트 변수를 구성한다. 템플릿 파일은 디폴트로 books/book_list.html 파일(앱이름/모델이름.html)이 된다.

장고가 알아서 해 주는 것?

{% raw %}

```python
class BookList(DetailView):
    model = Book
```

{% endraw %}

컨텍스트 변수로 object를 사용한다. 템플릿 파일은 디폴트로 books/book_detail.html 파일(앱이름/모델이름\_detail.html)이 된다. 테이블 조회 조건에 사용되는 Primary Key 값은 URLconf에서 넘겨받는데, 이에 대한 처리는 DetailView 제네릭 뷰에서 알아서 처리해 준다.

## 블로그 앱 만들기

title
modify_date
description
페이지 이동

#### 슬러그(slug)

페이지나 포스트를 설명하는 핵심 단어의 집합. 신문, 잡지 등에서 제목을 쓸 때, 중요한 의미를 포함하는 단어만 사용해 제목을 작성하는 방법을 말한다. 웹 개발 분야에서는 콘텐츠 고유 주소로 URL에 사용됨.

reverse() 장고 함수: URL 패턴을 만들어 주는 장고의 내장 함수

{% raw %}

```python
slug = models.SlugField('SLUG', unique=True, allow_unicode=True, help_text='one word for title alias.')

```

{% endraw %}

- unique=True: 특정 포스트를 검색할 때 기본 키 대신 사용된다.
- allow_unicode: 한글 처리 가능

DateTimeField의

- auto_now_add: 객체가 생성될 때의 시각을 자동으로 기록
- auto_now: 객체가 데이터베이스에 저장될 때의 시각을 자동으로 기록(즉, 변경될 때마다)

{% raw %}

```python
    class Meta:
        verbose_name = 'post'
        verbose_name_plural = 'posts'
        db_table = 'blog_posts'
        ordering = ('-modify_dt',)
```

{% endraw %}

- db*table = 'blog_posts': 데이터베이스에 저장되는 테이블 이름을 'blog_posts'로 저장한다. 디폴트로는 '앱명*모델클래스명'을 테이블 명으로 지정한다. 즉, 그대로 두면 blog_post가 되는데, blog_posts로 바꾼 것.

{% raw %}

```python
re_path(r'^post/(?P<slug>[-\w]+)/$', views.PostDV.as_view(), name='post_detail'),
```

{% endraw %}

파이썬에서는 정규표현식 그룹에서 숫자로 인덱싱하는 대신, 그룹이름으로 지정할 수 있다. (?P<그룹명>정규식)

{% raw %}

```python
class PostAV(ArchiveIndexView):
    model = Post
    date_field = 'modify_dt'
```

{% endraw %}

`modify_dt`를 기준으로 Post를 보여준다.

{% raw %}

```python
class PostYAV(YearArchiveView):
    model = Post
    date_field = 'modify_dt'
    make_object_list = True
```

{% endraw %}

make_object_list가 True이면 해당 연도에 해당하는 객체의 리스트를 만들어서 템플릿에 넘겨줍니다. 템플릿 파일에서는 object_list 컨텍스트 변수를 사용할 수 있습니다.

날짜를 "July 07, 2019" 형식으로 출력한다.

{% raw %}

```html
{{ post.modify_dt|date:"N d, Y" }}
```

{% endraw %}

템플릿 파일에서 URL을 추출하는 문법 2가지

- get_absolute_url()
- {% raw %}{% url %}{% endraw %}
