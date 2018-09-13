# 2. Install the Django Rest Framework

## Install Package

```commandline
pip install djangorestframework
pip install markdown
pip install django-filter
```

## Add app

```python
INSTALLED_APPS = []
    ...
    'rest_framework',
]
```

```commandline
python manage.py makemigrations
python manage.py migrate
```