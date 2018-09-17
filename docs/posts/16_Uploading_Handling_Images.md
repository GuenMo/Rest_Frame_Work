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