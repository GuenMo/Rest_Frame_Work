# 19. Global Settings for Authentication Permissions

> permission_classes, authentication_classes 의 셋팅 파일을 만든다.

## 폴더 및 파일 생성

```commandline
mkdir restapi/mysite/restconf

touch restapi/mysite/restconf/__init__.py
touch restapi/mysite/restconf/main.py
```

## main setting 파일 로드

```python
# settins.py

from mysite.restconf.main import *
```

## Default settings

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        # 'rest_framework.authentication.BasicAuthentication',
        'rest_framework.authentication.SessionAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ),
}
```

## View 확인

> 모든 api 의 views 파일에서 permission_classes, authentication_classes 주석 처리 한다.
