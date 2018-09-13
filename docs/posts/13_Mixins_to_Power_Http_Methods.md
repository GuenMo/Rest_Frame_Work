# 13. Mixins to Power Http Methods

>`StatusAPIView(List)`, `StatusCreateAPIView(Create)`
> `StatusDetailAPIView(Retrieve)`, `StatusUpdateAPIView(Update)`, `StatusDeleteAPIView(Destroy)`
>
> CRUD API를 구현 하기위해 위의 5개의 View를 구현 했다.
> 하지만 코드가 비슷하거나 같기도 하다.
> 그래서 5개의 클래스를 하나로 구현 한다.

## Create + List

```python
from rest_framework import mixins

class StatusAPIView(mixins.CreateModelMixin, generics.ListAPIView):
    permission_classes = []
    authentication_classes = []
    serializer_class = StatusSerializer

    def get_queryset(self):
        qs = Status.objects.all()
        query = self.request.GET.get('q')
        if query is not None:
            qs = qs.filter(content__icontains=query)
        return qs

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)

    # def perform_create(self, serializer):
    #     serializer.save(user=self.request.user)
```

## Detail + Update + Delete (id, pk, slug를 공통으로 필요로 한다.)

```python
from rest_framework import mixins

class StatusDetailAPIView(mixins.DestroyModelMixin, mixins.UpdateModelMixin, generics.RetrieveAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
    # lookup_field = 'slug'

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

## urls

```python
from django.urls import path

from .views import StatusAPIView, StatusDetailAPIView

urlpatterns = [
    path('', StatusAPIView.as_view()),
    path('<int:pk>/', StatusDetailAPIView.as_view()),
]
```

## Test1

<http://127.0.0.1:8000/api/status/>

<http://127.0.0.1:8000/api/status/8>

## Create + List = ListCreateAPIView

```python
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

## Detail + Update + Delete = RetrieveUpdateDestroyAPIView

```python
class StatusDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = []
    authentication_classes = []
    queryset = Status.objects.all()
    serializer_class = StatusSerializer
    # lookup_field = 'slug'
```

## Test2

<http://127.0.0.1:8000/api/status/>

<http://127.0.0.1:8000/api/status/8>