# 15. One API Endpoint for CRUD II

## Install 'requests'

```commandline
pip install requests
```

## 외부에서 API 접근 I

url에 id 정보를 포함해서 접근 한다.

```commandline
mkdir restapi/scripts
touch one_endpoints_test.py
```

```python
# on_endpoints_test.py

import requests

def do_one_endpoint(method='get', data={}, id=None):
    if id is not None:
        ENDPOINT = 'http://127.0.0.1:8000/api/status/' + '?id=' + str(id)
    else:
        ENDPOINT = 'http://127.0.0.1:8000/api/status/'
    r = requests.request(method, ENDPOINT, data=data)
    print(r.text)
    return r

do_one_endpoint(id=11)
# [{"id":8,"user":1,"content":"Update","image":null},{"id":11,"user":1,"content":"아아아아아","image":null}]
```

## 외부에서 API 접근 II

data에 id 정보를 포함해서 접근 한다.

```python
# views.py

import json
...
def get(self, request, *args, **kwargs):
    url_passed_id = request.GET.get('id', None)
    json_data = json.loads(request.body)
    data_passed_id = json_data.get('id', None)
    passed_id = url_passed_id or data_passed_id or None
    print(passed_id)
    if passed_id is not None:
        self.retrieve(request, *args, **kwargs)
    return super().get(request, *args, **kwargs)
```

```python
# on_endpoints_test.py

import requests
import json

def do_one_endpoint(method='get', data={}, id=None, is_json=True):
    if is_json:
        data = json.dumps(data)
    if id is not None:
        ENDPOINT = 'http://127.0.0.1:8000/api/status/' + '?id=' + str(id)
    else:
        ENDPOINT = 'http://127.0.0.1:8000/api/status/'
    r = requests.request(method, ENDPOINT, data=data)
    print(r.text)
    return r


do_one_endpoint(id=11)  # url_passed_id
do_one_endpoint(data={'id': 8})  # data_passed_id
do_one_endpoint(data={'id': 8}, id=11)  # url_passed_id & data_passed_id

# [{"id":8,"user":1,"content":"Update","image":null},{"id":11,"user":1,"content":"아아아아아","image":null}]
# [{"id":8,"user":1,"content":"Update","image":null},{"id":11,"user":1,"content":"아아아아아","image":null}]
# [{"id":8,"user":1,"content":"Update","image":null},{"id":11,"user":1,"content":"아아아아아","image":null}]

# 11
# 8
# 11
```