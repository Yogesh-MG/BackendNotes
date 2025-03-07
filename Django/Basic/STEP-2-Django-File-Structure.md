```shell
my_django_project/        # Project directory
    manage.py             # Command-line utility for Django project management
    my_django_project/    # Project package (same name as project directory)
        __init__.py       # Marks this directory as a Python package
        settings.py       # Settings/configuration for the Django project
        urls.py           # The URL declarations for the project
        asgi.py           # ASGI config
        wsgi.py           # WSGI config (used for deployment)

    my_app/               # Your Django app directory
        __init__.py       # Marks this directory as a Python package
        admin.py          # Admin interface configuration
        apps.py           # App configuration
        models.py         # Database models for your app
        views.py          # Request handlers (views) for your app
        urls.py           # URL routing for the app
        migrations/       # Database migrations for your models
            __init__.py
        static/           # Static files (CSS, JavaScript, images)
        templates/        # HTML templates for your app
        management/       # The schedule task command directory
		    commands/ 
			    run_task.py #The scheduled task file    
        tests.py          # Unit tests for your app

```
[Next](obsidian://open?vault=smart-india&file=Django%2FBasic%2FSTEP-3-Create%20a%20Project%20and%20app)
