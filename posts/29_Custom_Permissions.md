# 29. Custom Permissions

## AnonPermissionOnly

```commandline
touch resapi/accounts/api/permissions.py
```

```python
# permissions.py

from rest_framework import permissions

class AnonPermissionOnly(permissions.BasePermission):
    message = 'You are already authenticated. Please log out to try again.'
    def has_permission(self, request, view):
        return not request.user.is_authenticated
```

## Use Permission

```python
from .permissions import AnonPermissionOnly

class AuthView(APIView):
    permission_classes = [AnonPermissionOnly]
    ...

class RegisterAPIView(CreateAPIView):
    permission_classes = [AnonPermissionOnly]
    ...
```

## Test

> token을 받아서 유저 등록을 하려고 하면 **You are already authenticated. Please log out to try again.** 메시가나 뜨면서 유저 등록이 안된다. token을 받았다는 의미는 로그인이 되어 있다는 의미이고, RegisterAPIView는 로그인 되지 않은 사람에게 권한이 주어진다.

```python
import json
import requests

# GET TOKEN
AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/'
headers = {'Content-Type': 'application/json'}
data = {'username': 'admin', 'password': '1004kgm44'}
r = requests.post(AUTH_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()['token']

# Register
AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/'
REGISTER_ENDPOINT = AUTH_ENDPOINT + 'register/'
headers = {'Content-Type': 'application/json',
           'Authorization': 'JWT ' + token}
data = {'username': 'ximya1',
        'email': 'abc1@abc.com',
        'password': '1004kgm44',
        'password2': '1004kgm44'
        }

r = requests.post(REGISTER_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()
print(token)
```