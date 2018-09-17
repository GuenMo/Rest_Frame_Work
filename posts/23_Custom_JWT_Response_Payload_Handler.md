# 23. Custom JWT Response Payload Handler

> token 정보 외에 **user name** 과 **expires** 추가 정보를 추가 한다.

## Create app **accounts**

```conmandline
python manage.py startapp accounts
```

```python
# mysite/settings.py

INSTALLED_APPS = [
    ...
    'accounts'
]
```

```conmandline
python manage.py makemigrations
python manage.py migrate
```

## API 추가

```commandline
mkdir restapi/accounts/api
touch restapi/accounts/api/__init__.py
touch restapi/accounts/api/utils.py
```

```python
# utils.py

import datetime
from django.conf import settings
from django.utils import timezone

expire_delta = settings.JWT_AUTH['JWT_REFRESH_EXPIRATION_DELTA']


def jwt_response_payload_handler(token, user=None, request=None):
    return {
        'token': token,
        'user': user.username,
        'expires': timezone.now() + expire_delta - datetime.timedelta(seconds=200)
    }
```

```python
# mysite/resconf/main.py

...
'JWT_RESPONSE_PAYLOAD_HANDLER':
    # 'rest_framework_jwt.utils.jwt_response_payload_handler',
    'accounts.api.utils.jwt_response_payload_handler',
...
```

## API Test

```python
import json
import requests

AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/jwt/'
REFRESH_ENDPOINT = AUTH_ENDPOINT + 'refresh/'

headers = {'Content-Type': 'application/json'}
data = {'username': 'admin', 'password': '1004kgm44'}

r = requests.post(AUTH_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()['token']
user = r.json()['user']
expires = r.json()['expires']
print(token)
print(user)
print(expires)
```