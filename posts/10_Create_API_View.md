# 10. Create API View

## Create view

```python
# status/api/views.py

class StatusCreateAPIView(generics.CreateAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer

    # def perform_create(self, serializer):
    #     serializer.save(user=self.request.user)
```

## urls 추가

```python
# status/api/urls.py

from django.urls import path

from .views import StatusAPIView, StatusCreateAPIView

urlpatterns = [
    ...
    path('create/', StatusCreateAPIView.as_view()),
]
```

## Test

<http://127.0.0.1:8000/api/status/create/>
