# 6. Create & Update through Serializer

## Module import

```python
from status.models import Status
from status.api.srializers import StatusSerializer
```

## Create

```python
data = {'user': 1, 'content': '하이~~', 'image': None}
serializer = StatusSerializer(data=data)
if serializer.is_valid():
    serializer.save()
obj = Status.objects.last()
```

## Update

```python
obj = Status.objects.last()
data = {'user': 1, 'content': 'Hello'}
serializer = StatusSerializer(obj, data=data)
if serializer.is_valid():
    serializer.save()
obj = Status.objects.last()
```

## Delete

```python
# Create
data = {'user': 1, 'content': 'Delete me.'}
serializer = StatusSerializer(data=data)
if serializer.is_valid():
    serializer.save()
# Delete Error
obj = Status.objects.last()
serializer = StatusSerializer(obj)
serializer.delete() # Error
serializer.data.delete() # Error
# Delete
obj.delete()
```
