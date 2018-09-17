# 18. Authentication & Permissions

## Django Sessions

> authentication_classes 는 user 어떤 방식으로 인증 할 것인가를 결정 짓는다
> SessionAuthentication 란 페이지에 로그인 한 user로 인증 한다.

```python+theme:dark+lineNumbers:true
# views.py

from rest_framework.authentication import SessionAuthentication

class StatusAPIView(generics.ListCreateAPIView):

    permission_classes = []
    authentication_classes = [SessionAuthentication]
    serializer_class = StatusSerializer

    def get_queryset(self):
        qs = Status.objects.all()
        query = self.request.GET.get('q')
        if query is not None:
            qs = qs.filter(content__icontains=query)
        return qs

    def perform_create(self, serializer):
        # 로그인 되어 있는 유저를 user로 사용한다.
        serializer.save(user=self.request.user)
```

## Edit Seializer

```python+theme:dark+lineNumbers:true
# seializers.py

from rest_framework import serializers

from status.models import Status


class StatusSerializer(serializers.ModelSerializer):

    class Meta:
        model = Status
        fields = [
            'id',
            'user',
            'content',
            'image'
        ]
        read_only_fields = ['id', 'user']
...
```

## Permissions

> permission_classes 는 권한을 설정 한다. 
> 로그 아웃 상태에서는 StatusAPIView 에서 에러가 난다.
> perform_create 함수에서 user 객체가 정의 되지 않기 때문이다.
> IsAuthenticatedOrReadOnly 란 로그인 된 user에게 쓰기 읽기 권한을 주고 그렇지 않을 경우 읽기 권한만 준다.

```python+theme:dark+lineNumbers:true
from rest_framework import generics, permissions
from rest_framework.authentication import SessionAuthentication

from status.models import Status
from .srializers import StatusSerializer


class StatusAPIView(generics.ListCreateAPIView):

    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    authentication_classes = [SessionAuthentication]
    serializer_class = StatusSerializer

    def get_queryset(self):
        qs = Status.objects.all()
        query = self.request.GET.get('q')
        if query is not None:
            qs = qs.filter(content__icontains=query)
        return qs

    def perform_create(self, serializer):
        serializer.save(user=self.request.user)
```
