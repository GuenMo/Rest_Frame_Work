# 8. API Endpoint Overview

## url 추가

```python
# mysite/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/status/', include('status.api.urls')),
]
```


## View & Url 생성

```commandline
touch status/api/views.py
touch status/api/urls.py
```

1. Create, Retrive, Update, Delete 4개 view를 만든다.

