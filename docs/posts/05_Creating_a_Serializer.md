# 5. Creating a Serializer

## 폴더 및 파일 생성

```commandline
mkdir status/api
touch __init__.py
touch serializerpy
```

## Serialize a single object

```python
import json
from status.models import Status
from status.api.srializers import StatusSerializer
from rest_framework.renderers import JSONRenderer
from django.utils.six import BytesIO
from rest_framework.parsers import JSONParser

model_obj = Status.objects.first() # model object
serialized_obj = StatusSerializer(model_obj) # serialize object
serialized_obj.data # 딕셔너리랑 같은 형태의 데이터. 바로 Json 으로 바꿀수 없다.
json_data = JSONRenderer().render(serialized_obj.data) # bytes 데이터

# 두가지 방식
# 1
dict_data1 = json.loads(json_data) # 딕셔너리
# 2
stream = BytesIO(json_data)
dict_data2 = JSONParser().parse(stream) # 딕셔너리
```

## Serialize a queryset

```python
from status.models import Status
from status.api.srializers import StatusSerializer
from rest_framework.renderers import JSONRenderer
from django.utils.six import BytesIO
from rest_framework.parsers import JSONParser

qs = Status.objects.all()
serialized_objs = StatusSerializer(qs, many=True)
json_data = JSONRenderer().render(serialized_objs.data) # bytes 데이터
stream = BytesIO(json_data)
dict_data = JSONParser().parse(stream) # 딕셔너리
```