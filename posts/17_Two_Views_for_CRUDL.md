# 17. Two Views for CRUDL

## List & Create View

```python
# views.py

from rest_framework import generics
from status.models import Status
from .srializers import StatusSerializer


class StatusAPIView(generics.ListCreateAPIView):

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

## Retreive & Upload & Delete View

```python
# views.py

class StatusDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
```

## 외부에서 API 접근

```commandline
touch restapi/scripts/two_endpoint_test.py
```

```python
# two_endpoint_test.py


```
