# Step-1
<p>Define the Model field in the <span style="color:green;">models.py</span> which you have to save the Image url which can be accessed later</p>

```python models.py 
from django.db import models


class Imagesave(models.Model):
	title = models.Charfield(max_length=255)
	image = models.Imagefield(upload_to="images/")
	date = models.DateField(auto_now_add=True)

	def __str__(self):
		return self.title
```

# Step-2
<p>Register the Model in the admin.py </p>
```python 
from django.contrib import admin
from .models import Imagesave

admin.site.register(Imagesave)

```

# Step-3
<p>Now You want to access the image that you uploaded in the admin panel. For this we have make the changes in the <span style='color:green;'>views.py</span></p>
```python
from django.shortcut import render
from .models import Imagesave


def home(request):
	image = Imagesave.Object.all()
	return render(request, 'index.html', {'image':image})

```

# Step-4
<p>So Now we have the access to the image objects and we have rendered it to the <span style="color:green;" >index.html</span> </p>
```html
<section id="events" class="py-5 dark-background" style="margin-top:120px;margin-bottom:50px">
        <div class="container">
            <h2 class="text-center mb-5">Upcoming Events</h2>
            {% for gallery in image  %}
            <div class="row g-4">
                <div class="col-md-4">
                    <div class="event-card">
                        <img src="{{gallery.image.url}}" alt="Event 1"
		                      class="img-fluid">
                        <div class="p-4">
                            <h3>{{gallery.title}}</h3>
                            <p>{{gallery.description}}</p>
                            <h4>{{gallery.date}}</h4>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            {% endfor %}
        </div>
    </section>
```
cm