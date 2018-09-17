# 3. Status Model & App

## App 생성

```commandlined
python managy.py startapp status
```

## Model 생성

```python
# status/models.py

from django.db import models
from django.conf import settings


def upload_status_image(instance, filename):
    return "updateds/{user}/{filename}".format(user=instance.user, filename=filename)


class StatusQuerySet(models.QuerySet):
    pass


class StatusManager(models.Manager):
    def get_queryset(self):
        return StatusQuerySet(self.model, using=self._db)


class Status(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    content = models.TextField(null=True, blank=True)
    image = models.ImageField(upload_to=upload_status_image, null=True, blank=True)
    updated = models.DateTimeField(auto_now=True)
    timestamp = models.DateTimeField(auto_now_add=True)

    objects = StatusManager()

    def __str__(self):
        return str(self.content)[:50]

    class Meta:
        verbose_name = 'Status post'
        verbose_name_plural = 'Status posts'
```

## App 등록

```python
# mysite/settings.py

INSTALLED_APPS = [
    ...
    'status'
]
```

## Migration

```commandline
python manage.py makemigrations
python manage.py migrate
```

## Admin page 등록

```python
# status/admin.py

from django.contrib import admin

from .models import Status

admin.site.register(Status)

```