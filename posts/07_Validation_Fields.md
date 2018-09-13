# 7. Validation & Fields

## forms

```python
class StatusForm(forms.ModelForm):
    class Meta:
        model = Status
        fields = [
            'user',
            'content',
            'image'
        ]

    def clean_content(self, *args, **kwargs):
        content = self.cleaned_data.get('content')
        if len(content) > 240:
            raise forms.ValidationError('Content is too long.')
        return content

    def clean(self, *args, **kwargs):
        data = self.cleaned_data
        content = data.get('content', None)
        if content == '':
            content = None

        image = data.get('image', None)
        if content is None and image is None:
            raise forms.ValidationError('Content or image is required.')
        return super().clean(*args, **kwargs)
```

## serializer

```python
class StatusSerializer(serializers.ModelSerializer):
    class Meta:
        model = Status
        fields = [
            'user',
            'content',
            'image'
        ]

    def validate_content(self, value):
        if len(value) > 240:
            raise serializers.ValidationError('Content is too long.')
        return value

    def validate(self, data):
        content = data.get('content', None)
        if content == '':
            content = None

        image = data.get('image', None)
        if content is None and image is None:
            raise serializers.ValidationError('Content or image is required.')
        return data
```

1. clean_<field_name>(self, *args, **kwargs) ==> validate_<filed_name>(self, value)
2. content = self.cleaned_data.get('content') ==> value
3. forms.ValidationError ==> serializer.ValidationError
4. data = self.cleaned_data ==> data
5. return super().clean(*args, **kwargs) ==> return data

## Test

```python
from status.api.srializers import StatusSerializer

# 콘텐트와 이미지가 없이 생성 할 때 에러 확인
data = {'user': 1, 'content': '', 'image': None}
serializer = StatusSerializer(data=data)
serializer.is_valid()
# Result: False
serializer.errors
serializer.error_messages

# 콘텐트 내용이 240자 이상 일 때 에러 확인
data = {'user': 1,'content': 'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa','image': None}
serializer = StatusSerializer(data=data)
serializer.is_valid()
# Result: False
serializer.errors
serializer.error_messages
```