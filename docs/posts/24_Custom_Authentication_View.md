# 24. Custom Authentication View

> main urls.py에 auth 과련 url을 독립 시킨다.

## Main urls.py

```python
# mysite/urls.py

from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/auth/', include('accounts.api.urls')),
    path('api/status/', include('status.api.urls')),
]

if settings.DEBUG:
    urlpatterns = urlpatterns + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns = urlpatterns + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## API View

```commandline
touch restapi/accounts/api/urls.py
touch restapi/accounts/api/views.py
```

```python
# restapi/accounts/api/urls.py

from django.urls import path
from .views import APIView
from rest_framework_jwt.views import obtain_jwt_token, refresh_jwt_token

urlpatterns = [
    path('', APIView.as_view()),
    path('jwt/', obtain_jwt_token),
    path('jwt/refresh/', refresh_jwt_token),
]
```

```python
# restapi/accounts/api/views.py

from rest_framework.views import APIView
from rest_framework.response import Response


class AuthView(APIView):
    def post(self, request, *args, **kwargs):
        return Response({'token': 'abc'})
```
