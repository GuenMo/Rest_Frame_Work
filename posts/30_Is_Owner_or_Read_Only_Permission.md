# 30. Is Owner or Read Only Permission

## IsOwnerOrReadOnly

```python
# permissions.py

class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    Object-level permission to only allow owners of an object to edit it.
    Assumes the model instance has an `owner` attribute.
    """

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Instance must have an attribute named `owner`.
        return obj.owner == request.user
```

## Use Permission

```python
# restapi/status/api/views.py

from accounts.api.permissions import IsOwnerOrReadOnly
...
class StatusDetailAPIView(generics.RetrieveUpdateDestroyAPIView):
    permission_classes = [permissions.IsAuthenticatedOrReadOnly, IsOwnerOrReadOnly]
    ...
```

## Test

> 1. 두명의 유저생성
> 2. 한명의 유저로 status 생성
> 3. 다른 한명의 유저가 Retrieve, Update, Delete 실행

```python
import json
import requests

# Register
AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/'
REGISTER_ENDPOINT = AUTH_ENDPOINT + 'register/'
headers = {'Content-Type': 'application/json'}
data1 = {'username': 'user1', 'email': 'user1@abc.com', 'password': '1004kgm44', 'password2': '1004kgm44'}
data2 = {'username': 'user2', 'email': 'user2@abc.com', 'password': '1004kgm44', 'password2': '1004kgm44'}

r = requests.post(REGISTER_ENDPOINT, data=json.dumps(data1), headers=headers)
token = r.json()
print(token)

r = requests.post(REGISTER_ENDPOINT, data=json.dumps(data2), headers=headers)
token = r.json()
print(token)

# GET TOKEN
AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/'
headers = {'Content-Type': 'application/json'}
data = {'username': 'user1', 'password': '1004kgm44'}
r = requests.post(AUTH_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()['token']
print(token)

# CREATE
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
headers = {'Authorization': 'JWT ' + token}
post_data = {'content': 'Create from API using token'}
post_r = requests.post(ENDPOINT, data=post_data, headers=headers)
print(post_r.text)

# GET TOKEN
AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/'
headers = {'Content-Type': 'application/json'}
data = {'username': 'user1', 'password': '1004kgm44'}
r = requests.post(AUTH_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()['token']
print(token)

# UPDATE
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
headers = {'Authorization': 'JWT ' + token}
update_data = {'content': 'Update from API using token'}
update_r = requests.put(ENDPOINT + str(43) + '/', data=update_data, headers=headers)
print(update_r.text)

# RETRIEVE
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
headers = {'Authorization': 'JWT ' + token}
detail_r = requests.get(ENDPOINT + str(43) + '/', headers=headers)
print(detail_r.text)

# DELETE
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
headers = {'Authorization': 'JWT ' + token}
delete_r = requests.delete(ENDPOINT + str(43) + '/', headers=headers)
print(delete_r.status_code)
```