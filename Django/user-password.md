# Django User password 변경  
## console 이용하기  
```python
from django.contrib.auth.models import User
user = User.objects.last()
user.set_password('새로운 비밀번호')
user.save()
```

## User 아이디를 알고있는 경우  
```bash
$ python manage.py changepassword 유저아이디
```

아니면 Admin 페이지에서 변경해도 된다.
