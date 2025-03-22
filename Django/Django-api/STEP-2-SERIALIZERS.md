<h4>Create a new python file <span style="color:green">serializers.py</span> in the my_app directory</h4>

`serializers.py`
```python
from rest_framework import serializers
from .models import Users

class UserSerializers(serializers.ModelSerializer):
	class Meta:
		model=Users
		field='__all__'
```

- in this we imported the data from the database which we want to transfer as API with the help of  rest_framework 

<h4>Define the function in <span style="color:green">views.py</span> to convert the object form data into JSON format</h4>
`views.py`
```python
from rest_framework import viewsets
from .models import User
from .serializers import UserSerializer


class UsersViewset(viewsets.ReadOnlyModelViewSet):
	queryset = Users.objects.all()
	serializers_class = UserSerializer
	
	
```

<h4>Allowing the API data-accesing by routing the views function in the urls.py</h4>
`urls.py`
```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import UsersViewset

router = DefaultRouter()
router.register(r'user', UsersViewset, basename='user')

urlpatterns = [
	path('api/', include(router.urls)),
]
```


<h5>python manage.py runserver</h5>
Now, you can access:

- `http://127.0.0.1:8000/api/user/`
