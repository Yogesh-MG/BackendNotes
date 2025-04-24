<h3>Things should be installed before moving to API setup</h3>  
- So make sure you are inside the virtual-environment (if you don't know then refer **[[STEP-1-WHY-DJANGO]]** ) and Install Graphene-Django, which integrates GraphQL with Django, and any dependencies.


```shell
(env)$pip install graphene-django
```

- After installation make the bellow changes in the **settings.py**. 

`settings.py`
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',  #include this
    'api',
    'corsheaders',     #include this
    'graphene_django'
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',    #include this as first element
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

#in development server
CORS_ALLOW_ALL_ORIGINS = True 

#in proudtion
CORS_ALLOWED_ORIGINS = [
    "http://localhost:8080",  # Allow requests from Vite/React frontend
    "http://127.0.0.1:3000",  # Allow React Dev Server
    "https://yourfrontend.com",  # Your production frontend domain
]

GRAPHENE = {
	'SCHEMA': 'project_root_folder.schema.schema'
}

```

- As the next step should be defining the database (if you don't know how then refer                 this  **[[STEP-4-DATAMODEL & settings setup]]** )
 


[NEXT](obsidian://open?vault=Notes-of-All&file=Django%2FDjango-graphene-api%2FSTEP-2-Schema-For-Graphene)
