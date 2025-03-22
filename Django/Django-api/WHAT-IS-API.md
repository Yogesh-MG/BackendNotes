<h4>API also known as Application Programming Interface</h4>
<p>Is a set of rules and tools that allows different software application to communicate with each other. It allows one program to send request to another program without needing to understanding its internal working.</p>

<h5>How it is used in Django</h5>
<p>In Django, an API typically refers to a way t expose your Django application data or functionality to other applications, often over web. Django, being a Python web framework, makes it straightforward to build APIs.</p>

<h5>How APIs works in Django</h5>
- Django REST Framework (DRF): It's a powerful toolkit built on top of Django, it simplifies creating the APIs by providing tools like serializers (Which converts the data between Python objects and formats like JSON), views, and authentication.
- Basics steps to follow:
   - Step1-Define the **Models** in <span style="color:green">models.py</span> to represent the data
   - Step2-Use **serializers** in <span style="color:green">serializer.py</span> for data conversion
   - Step3-Create **views** function in <span style="color:green">views.py</span> to handle API request and response
   - Step4-Map these in **URLs** in <span style="color:green">urls.py</span> so they are accessible at endpoints like **/api/users/**.
