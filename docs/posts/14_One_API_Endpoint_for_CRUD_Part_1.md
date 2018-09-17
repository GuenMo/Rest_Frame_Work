# 14. One API Endpoint for CRUD I

>`StatusAPIView(List)`, `StatusCreateAPIView(Create)`
> `StatusDetailAPIView(Retrieve)`, `StatusUpdateAPIView(Update)`, `StatusDeleteAPIView(Destroy)`
>
> CRUD API를 구현 하기위해 위의 5개의 View를 구현 했다.
> 하지만 코드가 비슷하거나 같기도 하다.
> 그래서 5개의 클래스를 하나로 구현 한다.

## One API Endpoint for CRUD

```python
from rest_framework import generics
from rest_framework import mixins
from status.models import Status
from django.shortcuts import get_object_or_404
from .srializers import StatusSerializer


class StatusAPIView(mixins.CreateModelMixin,
                    mixins.UpdateModelMixin,
                    mixins.DestroyModelMixin,
                    mixins.RetrieveModelMixin,
                    generics.ListAPIView):

    permission_classes = []
    authentication_classes = []
    serializer_class = StatusSerializer

    def get_queryset(self):
        print('get_queryset')
        qs = Status.objects.all()
        query = self.request.GET.get('q')
        if query is not None:
            qs = qs.filter(content__icontains=query)
        return qs

    def get_object(self):
        request = self.request
        passed_id = request.GET.get('id', None)
        print('get_object', passed_id)
        queryset = self.get_queryset()
        obj = None
        if passed_id is not None:
            obj = get_object_or_404(queryset, id=passed_id)
            self.check_object_permissions(request, obj)
        print(obj)
        return obj

    def get(self, request, *args, **kwargs):
        passed_id = request.GET.get('id', None)
        print('get', passed_id)
        if passed_id is not None:
            self.retrieve(request, *args, **kwargs)
        return super().get(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
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
]
```

## Test

<http://127.0.0.1:8000/api/status/>

get_queryset 함수를 호출 한다.

<http://127.0.0.1:8000/api/status/?id=8>

get -> get_object -> get_queryset 함수를 차례로 호출 한다.