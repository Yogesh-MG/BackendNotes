After the install process of DBMS 

step 1: open settings.py of project file 
```python 
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'gps',
        'USER': 'postgres',
        'PASSWORD': '990044',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

change will be applicable if the database name **gps** of owner name **postgres**.
step 2: Migrate the database
```shell
$ python3 manage.py makemigrations
$ python3 manage.py migrate
```
