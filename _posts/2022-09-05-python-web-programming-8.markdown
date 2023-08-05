---
layout: post
title: "파이썬 웹 프로그래밍 공부 8"
date: 2022-09-05 22:21:52 +0900
tags: python-web-programming
---

## 프로젝트 첫 페이지 만들기

{% raw %}

```python
from django.views.generic.base import TemplateView
from django.apps import apps

class HomeView(TemplateView):
    template_name = 'home.html'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        # context['app_list'] = ['polls', 'books']
        dictVerbose = {}
        for app in apps.get_app_configs():
            if 'site-packages' not in app.path:
                dictVerbose[app.label] = app.verbose_name
        context['verbose_dict'] = dictVerbose
        return context
```

{% endraw %}

`get_app_configs()` 메서드는 settings.py 파일의 `INSTALLED_APPS`에 등록된 각 앱의 설정 클래스를 담은 리스트를 반환한다.

`if 'site-packages' not in app.path:`

- app: 각 앱의 설정 클래스.
- app.path: 애플리케이션 물리적 경로(C:\RedBook\ch5\books)
- `if 'site-packages' not in`: 'site-packages' 문자열이 들어 있으면 외부 라이브러리앱이므로 이 앱을 제외한다.

polls/views.py
{% raw %}

```python
from django.views import generic


class IndexView(generic.ListView): # def index(request):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
         return Question.objects.all().order_by('-pub_date')[:5]


class DetailView(generic.DetailView): #def detail(request, question_id):
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'
```

{% endraw %}

- 테이블에 들어 있는 모든 레코드를 가져와 구성하는 경우에는 테이블명, 즉 모델 클래스명만 지정해 주면 된다.

- DetailView를 상속받는 경우 특정 객체 하나를 컨텍스트 변수에 담아서 템플릿 시스템에 넘겨 주면 된다. 특정 객체를 테이블에서 Primary Key로 조회해서 가져오는 경우에는 테이블명, 즉 모델 클래스명만 지정해주면 됩니다.
