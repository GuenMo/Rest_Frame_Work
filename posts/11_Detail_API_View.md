# 11. Detail API View

## urls 추가

```python
# status/api/urls.py

from django.urls import path

from .views import StatusAPIView, StatusCreateAPIView, StatusDetailAPIView

urlpatterns = [
    ...
    path('<int:pk>/', StatusDetailAPIView.as_view()),
]
```

## Create view

```python
# status/api/views.py

class StatusDetailAPIView(generics.RetrieveAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
    # lookup_field = 'slug'
```

## Test

<http://127.0.0.1:8000/api/status/5s/>
