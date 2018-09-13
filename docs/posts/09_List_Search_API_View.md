# 9. List & Search API View

## urls 추가

```python
# status/api/urls.py

from django.urls import path

from .views import StatusAPIView

urlpatterns = [
    path('', StatusAPIView.as_view()),
]
```

## List view

```python
# status/api/views.py

from rest_framework import generics

from status.models import Status
from .srializers import StatusSerializer


class StatusAPIView(generics.ListAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
```

## List & Search view

```python
class StatusAPIView(generics.ListAPIView):
    permission_classes = []
    authentication_classes = []
    serializer_class = StatusSerializer

    def get_queryset(self):
        qs = Status.objects.all()
        query = self.request.GET.get('q')
        if query is not None:
            qs = qs.filter(content__icontains=query)
        return qs
```

## Test

[List](http://127.0.0.1:8000/api/status/)
[Search](http://127.0.0.1:8000/api/status/?q=new)