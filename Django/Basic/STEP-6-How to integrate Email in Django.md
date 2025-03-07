<h1>Step1</h1>
In Settings.py add
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

EMAIL_HOST = 'smtp.gmail.com'

EMAIL_PORT = 587

EMAIL_USE_TLS = True

EMAIL_HOST_USER = 'add your source email'

EMAIL_HOST_PASSWORD = 'get the password from google apps'
```
<h1>Step2</h1>
in the app where you want to integrate 
```python
from django.core.mail import send_mail

def send_email(request):
    if request.method == 'POST':
        email_id = request.POST.get('email')
        name = request.POST.get('name')
        book = request.POST.get('book')
        if email_id and name and book:  # Ensure all fields are provided
            try:
                subject = "The Book has not been returned by user"
                message = f"""
                Hello {name},
                You have not submitted the book "{book}" to the library. Kindly return the book.
                Warm regards,
                Library Management
                PALMS
                """
                from_email = 'yoge843120@gmail.com'
                recipient_list = [email_id]  # Ensure this is a list
                send_mail(subject, message=message, from_email=from_email, recipient_list=recipient_list)
            except Exception as e:

                print(f"Error occurred: {e}")
        else:
            print("Missing required fields: email, name, or book")
    return render(request, 'send_email.html')
```
<h1>Step3</h1>
In urls.py define the url to access
```python
from django.urls import path
from . import views

urlpatterns = [
    path('send_email/', views.send_email, name='email'),
]
```
