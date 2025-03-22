<h3>Things should be installed before moving to API setup</h3>  
- So make sure you are inside the virtual-environment (if you don't know then refer **[[STEP-1-WHY-DJANGO]]** ) and install the DRF (django-rest-framework) Which is play a a major role in the setup


```shell
(env)$pip install djangorestframework
(env)$pip install django-cors-headers
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

```

- As the next step should be defining the database (if you don't know how then refer this **[[STEP-4-DATAMODEL & settings setup]]** )
- Before moving to the next step make sure this points are fullfiled
  - <span style="color:green">Package installation</span>
  - <span style="color:green">Changes in the settings.py</span>
  - <span style="color:green">Database scheme defined in the models.py</span>
  - <span style="color:green">makemigrations and migrate</span>


<span style="color:green">If the above steps are cleared check this</span>. **[[STEP-2-SERIALIZERS]]**
