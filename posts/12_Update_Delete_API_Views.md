# 12. Update & Delete API Views

## Create view

```python
# status/api/views.py

class StatusUpdateAPIView(generics.UpdateAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
```

## Delete view

```python
# status/api/views.py

class StatusDeleteAPIView(generics.DestroyAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
```

## urls 추가

```python
# status/api/urls.py

from django.urls import path

from .views import StatusAPIView, StatusCreateAPIView, StatusDetailAPIView, StatusUpdateAPIView, StatusDeleteAPIView

urlpatterns = [
    path('', StatusAPIView.as_view()),
    path('create/', StatusCreateAPIView.as_view()),
    path('<int:pk>/', StatusDetailAPIView.as_view()),
    path('<int:pk>/update/', StatusUpdateAPIView.as_view()),
    path('<int:pk>/delete/', StatusDeleteAPIView.as_view()),
]
```

## Test

<http://127.0.0.1:8000/api/status/5/update/>

<http://127.0.0.1:8000/api/status/5/delete/>