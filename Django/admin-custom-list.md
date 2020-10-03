# Admin에 list-display 커스텀하기  

## 정렬 추가하기  
기본적인 컬럼에는 자동적으로 오름차순/내림차순이 작동하는데, 적용이 안 되어있는 컬럼에 임의로 정렬을 추가할 수 있다.  
### models.py  
```python
class Post(models.Model):
    ...
    
    liked_users = models.ManyToManyField(User, blank=True, related_name="liked_users", through="Like")

    @property
    def like_count(self):
        return self.liked_users.count()
```
`Post` 모델의 `liked_users`(좋아요) 개수 순으로 Admin에서 정렬을 하고  
```python
class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    name = models.CharField(verbose_name='name', max_length=10)
    
    ...
```
`User`와 1:1로 연결된 `Profile` 모델에 저장된 값을 가져오려면  

### admin.py  
```python
from django.contrib import admin
from .models import *

@admin.register(Post)
class PostAdmin(admin.ModelAdmin):
    list_display = (
        "id",
        "user",
        "get_user_name",
        "category",
        "title",
        "number_of_likes",
    )

    def number_of_likes(self, obj):
        return obj.like_count
    number_of_likes.admin_order_field = 'like'

    def get_user_name(self, obj):
        return obj.user.profile.name
    get_user_name.short_description = 'User Name'
```
정렬이 필요한 경우 `admin_order_field`를, 연결된 테이블의 값은 함수를 따로 만들어서 리턴하게 할 수 있다.

