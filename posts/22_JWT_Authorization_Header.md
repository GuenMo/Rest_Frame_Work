# 22. JWT Authorization Header

## API Test

```python
import json
import requests

AUTH_ENDPOINT = 'http://127.0.0.1:8000/api/auth/jwt/'
REFRESH_ENDPOINT = AUTH_ENDPOINT + 'refresh/'
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
image_path = 'd:/mai.jpg'

headers = {
    'Content-Type': 'application/json'
}

# Get Token
data = {'username': 'admin', 'password': '1004kgm44'}

r = requests.post(AUTH_ENDPOINT, data=json.dumps(data), headers=headers)
token = r.json()['token']
print(token)


# Retrieve
# 1. List
headers = {'Authorization': 'JWT ' + token}
list_r = requests.get(ENDPOINT, headers=headers)
print(list_r.text)
# 2. Detail
headers = {'Authorization': 'JWT ' + token}
detail_r = requests.get(ENDPOINT + str(36) + '/', headers=headers)
print(detail_r.text)

# Create
# 1. No image
headers = {'Authorization': 'JWT ' + token}
post_data = {'content': 'Create from API using token'}

post_r = requests.post(ENDPOINT, data=post_data, headers=headers)
print(post_r.text)

# 2. With image
headers = {'Authorization': 'JWT ' + token}
post_data = {'content': 'Create from API using token with image.'}

with open(image_path, 'rb') as image:
    file_data = {'image': image}
    post_r = requests.post(ENDPOINT, data=post_data, headers=headers, files=file_data)

# Update
# 1. No image
headers = {'Authorization': 'JWT ' + token}
update_data = {'content': 'Update from API using token'}

update_r = requests.put(ENDPOINT + str(37) + '/', data=update_data, headers=headers)
print(update_r.text)

# 2. With image
headers = {'Authorization': 'JWT ' + token}
update_data = {'content': 'Update from API using token with image'}

with open(image_path, 'rb') as image:
    file_data = {'image': image}
    update_r = requests.put(ENDPOINT + str(36) + '/', data=update_data, headers=headers, files=file_data)
print(update_r.text)

# Delete
headers = {'Authorization': 'JWT ' + token}
delete_r = requests.delete(ENDPOINT + str(36) + '/', headers=headers)
print(delete_r.status_code)
```
