# 18. Authentication & Permissions

## Django Sessions

> 페이지에 로그인 한 유저의 이름을 알아 낸다.

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


    def validate_content(self, value):
        if len(value) > 240:
            raise serializers.ValidationError('Content is too long.')
        return value

    def validate(self, data):
        content = data.get('content', None)
        if content == '':
            content = None

        image = data.get('image', None)
        if content is None and image is None:
            raise serializers.ValidationError('Content or image is required.')
        return data
```