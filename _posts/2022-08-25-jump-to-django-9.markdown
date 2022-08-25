---
layout: post
title:  "점프 투 장고 공부 9"
date:   2022-08-25 22:19:55 +0900
categories: jumptodjango
---

### 수정과 삭제

models.py
{% raw %}
```python
class Question(models.Model):
    subject = models.CharField(max_length=200)
    content = models.TextField()
    create_date = models.DateTimeField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    modify_date = models.DateTimeField(null=True, blank=True)

    def __str__(self):
        return self.subject


class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    content = models.TextField()
    create_date = models.DateTimeField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    modify_date = models.DateTimeField(null=True, blank=True)
```
{% endraw %}



#### 질문의 수정 버튼 추가

question_detail.html

{% raw %}
```html
<div class="my-3">
{% if request.user == question.user %}
    <a href="{% url pybo:question_modify question.id %}" class="btn btn-sm btn-outline-secondary">수정</a>
{% endif %}
</div>
```
{% endraw %}


urls.py
{% raw %}
```python
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('answer/create/<int:question_id>/', views.answer_create, name='answer_create'),
    path('question/create/', views.question_create, name='question_create'),
    path('question/modify/', views.question_modify, name='question_modify'),
]
```
{% endraw %}


views.py
{% raw %}
```python

@login_required(login_url='common:login')
def question_modify(request):
    question = get_object_or_404(Question, pk=question_id)
    if request.user != question.user:
        message.error(request, '수정권한이 없습니다.')
        return redirect('pybo:detail', question_id=question.id)
    if request.method == 'POST':
        form = QuestionForm(request.POST, instance=question)
        if form.is_valid():
            question = form.save(commit=False)
            question.modify_date = timezone.now()
            question.save()
            return redirect('pybo:detail', question_id=question_id)
    else:
        form = QuestionForm(instance=question)
    context = {'form': form}
    return render(request, 'pybo/question_form.html', context)

```
{% endraw %}







