# 16. Uploading & Handling Images

## Upload location

```python
# status/models.py
def upload_status_image(instance, filename):
    return "status/{user}/{filename}".format(user=instance.user, filename=filename)
```

```python
# mysite/settings.py
MEDIA_ROOT = os.path.join(BASE_DIR, 'media_root')
MEDIA_URL = '/media/'
```

```python
# mysite/urls.py
from django.conf import settings
from django.conf.urls.static import static
...
if settings.DEBUG:
    urlpatterns = urlpatterns + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
    urlpatterns = urlpatterns + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

## Create

```python
import json
import requests


def do_img(method='get', data={}, image_path=None, endpoint=None):
    headers = {}
    if image_path is not None:
        with open(image_path, 'rb') as image:
            file_data = {'image': image}
            r = requests.request(method, endpoint, data=data, headers=headers, files=file_data)
    else:
        headers['content-type'] = 'application/json'
        data = json.dumps(data)
        r = requests.request(method, endpoint, data=data, headers=headers)
    print(r.text)
    print(r.status_code)
    return r

# Create
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
data = {'user': 1, 'content': 'Create from external python'}
image_path = 'd:/mai.jpg'

# 1. No image
do_img(method='post', data=data, endpoint=ENDPOINT)
# 2. With image
do_img(method='post', data=data, image_path=image_path, endpoint=ENDPOINT)
```

## List

```python
ENDPOINT = 'http://127.0.0.1:8000/api/status/'
do_img(method='get', endpoint=ENDPOINT)
```

## Detail

```python
# Detail
ENDPOINT = 'http://127.0.0.1:8000/api/status/23/'
do_img(method='get', endpoint=ENDPOINT)
```

## Update

```python
ENDPOINT = 'http://127.0.0.1:8000/api/status/23/'
data = {'user': 1, 'content': 'Updata from external python with image.'}
image_path = 'd:/astellia.jpg'

# 1. No image
do_img(method='put', data=data, endpoint=ENDPOINT)
# 2. With image
do_img(method='put', data=data, image_path=image_path, endpoint=ENDPOINT)
```

## Delete

```python
# Delete
ENDPOINT = 'http://127.0.0.1:8000/api/status/23/'
do_img(method='delete', endpoint=ENDPOINT)
```