After following the [Step1](obsidian://open?vault=Notes-of-All&file=Django%2FDjango-graphene-api%2FSTEP-1-Package-Installation), Assuming you have defined the models.py, we have to create the schema directory, to organize it we have to create a folder in the root directory [check out](obsidian://open?vault=Notes-of-All&file=Django%2FBasic%2FSTEP-2-Django-File-Structure), after this define the schema with respect to the models.py

### 1) Set The App Level Schema
`graph_ql/schema.py`
```python
import graphene
from graphene_django import DjangoObjectType
from myapp.models import User



class UserType(DjangoObkectType):
	class Meta:
		model = User
		fields = ('id', 'name', 'age', 'email')

class Query(graphene.ObjectType):
	all_user = graphene.List(UserType)
	user_by_id = graphene.Field(UserType, id=graphene.Int(required=True))

	def resolve_all_user(self, name):
		return User.objects.all()

	def resolve_user_by_id(self, name, id)
		return User.objects.get(id=id)


schema = graphene.Schema(query=Query)
```
Define the schema with respect to the [models.py](obsidian://open?vault=Notes-of-All&file=Django%2FBasic%2FSTEP-4-DATAMODEL%20%26%20settings%20setup), so that graphene will have know what data to be sent.

### 2) Set Project Level Schema
`my_django_project/schema.py`  
```python
import graphene
import graph_ql.schema

class Query(graph_ql.schema.Query, graphene.ObjectType)
	pass

schema = graphene.Schema(query=Query)
```

`myapps/urls.py`
```python
from django.urls import path 
from . import views 
from graphene_django.views import GraphQLView
from my_django_project.schema import schema

urlpatterns = [ 
	
	path('add_user/', views.add_user, name='add_user'), 
	path('graphql/', GraphQLView.as_view(graphiql=True, schema=schema)),
	
]
```
