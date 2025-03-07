`
- In views.py`
```python
from django.shortcuts import render, redirect
from .forms import FillForm, NewsletterForm
from django.contrib import messages
# Create your views here.

def base(request):
    if request.method == "POST":
        if request.POST.get('form_type') == 'form1':
            form = FillForm(request.POST)
            print(form)
            if form.is_valid():
                form.save()
                messages.success(request, "You have submitted sucessfull")
                return redirect('home')
            else:
                messages.error(request, 'failed')
        elif request.POST.get('form_type') == 'form2':
            form = NewsletterForm(request.POST)
            if form.is_valid():
                form.save()
                return redirect('home')
            else:
                messages.error(request, 'failed')
    return render(request, 'home.html')
```

`
- In forms.py`

```python
from django import forms
from .models import Newsletter, Fillup

class FillForm(forms.ModelForm):
    class Meta:
        model = Fillup
        fields = ['Name', 'Email', 'Service', 'Message']
class NewsletterForm(forms.ModelForm):
    class Meta:
        model = Newsletter
        fields = ['Email']
```

- In html
```html
<form method="POST" id="">
	{% csrf_token %}
	<div class="input-group">
		<input type="email" name="Email" class="form-control bg-light border-              0" placeholder="Your Email*" style="height: 55px;" required>
		<button type="submit" class="btn btn-dark" name="form_type"                        value="form2">Sign Up</button>
	</div>
</form>


<form method="POST" id="">
	{% csrf_token %}
	<div class="row g-3">
		<div class="col-xl-12">
			<input type="text" name="Name" class="form-control bg-light                    border-0" placeholder="Your Name" style="height: 55px;" required>
			</div>
			<div class="col-12">
			<input type="email" name="Email" class="form-control bg-light                  border-0" placeholder="Your Email*" style="height: 55px;" required>
			</div>
			<div class="col-12">
			<select name="Service" class="form-select bg-light border-0"                     style="height: 55px;" required>
			<option selected disabled>Select A Service</option>
			<option value="Kunigal">Kunigal</option>
			</select>
			</div>
			<div class="col-12">
			<textarea name="Message" class="form-control bg-light border-0"                 rows="3" placeholder="Message" required></textarea>
			</div>
			<div class="col-12">
			<button class="btn btn-dark w-100 py-3" type="submit"                           name='form_type' value='form1'>Request A Quote</button>
		*You will receive notifications and offers related to our services and products.
		</div>
	</div>
</form>
```

- In models
```python
from django.db import models

class Fillup(models.Model):
    Name = models.CharField(max_length=50)
    Email = models.EmailField(unique=True, blank=False)
    Service = models.CharField(max_length=100)
    Message = models.CharField(max_length=255)
    def __str__(self):
        return f"{self.Name} -- {self.Email} -- {self.Service}"
class Newsletter(models.Model):
    Email = models.EmailField(blank=False, unique=True)
```