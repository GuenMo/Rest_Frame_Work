# 18. Authentication & Permissions

## Django Sessions

> 페이지에 로그인 한 유저의 이름을 알아 낸다.

```python+theme:dark+lineNumbers:true+lineNumberStart:8
# views.py

from rest_framework.authentication import SessionAuthentication
...
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
        print(self.request.user)
        serializer.save(user=self.request.user)
```