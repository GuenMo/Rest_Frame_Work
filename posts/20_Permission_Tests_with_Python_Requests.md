# 20. Permission Tests with Python Requests

## Test

> Create 코드에서 에러나 난다.

```python
import json
import requests

ENDPOINT = 'http://127.0.0.1:8000/api/status/'
image_path = 'd:/mai.jpg'

# Detail
get_endpoint = ENDPOINT + str(20)
r = requests.get(get_endpoint)
print(r.text)

# List
r2 = requests.get(ENDPOINT)
print(r2.text)
print(r2.status_code)

# Create
post_data = json.dumps({'content': 'Some random content'})
post_headers = {
    'content-type': 'application/json'
}
post_response = requests.post(ENDPOINT, post_data, headers=post_headers)
print(post_response.text)
```

## OAouth2

<http://www.sauru.so/blog/basic-of-oauth2-and-jwt/>

## Json Web Token(JWT)