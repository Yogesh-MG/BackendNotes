<h4>Create a new python file <span style="color:green">serializers.py</span> in the my_app directory</h4>

`serializers.py`
```python
from rest_framework import serializers
from .models import Users

class UserSerializers(serializers.ModelSerializer):
	class Meta:
		model=Users
		field='__all__' or ['id', 'username', 'password']
```

- in this we imported the data from the database which we want to transfer as API with the help of  rest_framework 
- So in the field section if you only want to transfer certain data from the database then mention only those to transfer through api

<h4>Define the function in <span style="color:green">views.py</span> to convert the object form data into JSON format</h4>
`views.py`
```python
from rest_framework import generics
from .models import User
from .serializers import UserSerializer


class UsersViewset(generics.ListAPIView):
	queryset = Users.objects.all()
	serializers_class = UserSerializer
	
	
```

- <span style='color:green'>generics</span> make you write less code cause you will have predefined sets so leverage it
- <span style='color:green'>queryset</span> defines what data you want to send through {} this format 
- <span style='color:green'>serializers_class</span> defines what and all data from the <span style='color:green'>queryset</span> to be sent 

<h4>Allowing the API data-accesing by routing the views function in the urls.py</h4>
`urls.py`
```python
from django.urls import path, include
from .views import UsersViewset

urlpatterns = [
	path('api/', UserViewset.as_view(), name='user-list'),
]
```

<h4>To recive the data from the frontend</h4>
`models.py`
```python
from django.db import models
# Create your models here.
  
class ContactSubmission(models.Model):
    name = models.CharField(max_length=200)
    email = models.EmailField()
    phone = models.CharField(max_length=15, null=True, blank=True)
    interested = models.CharField(max_length=200)
    location = models.CharField(max_length=200)
    message = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    def __str__(self):
        return f'{self.name} - {self.phone}'
```

`serializers.py`
```python
from rest_framework import serializers
from .models import ContactSubmission
  
class ContactSubmissionSerializer(serializers.ModelSerializer):
    class Meta:
        model = ContactSubmission
        fields = '__all__'

```

`views.py`
```python
from rest_framework.response import Response
from rest_framework.decorators import api_view
from rest_framework import status
from .models import ContactSubmission
from .serializers import ContactSubmissionSerializer
  
@api_view(['POST'])
def submit_contact(request):
    serializer = ContactSubmissionSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save()
        return Response({'message': 'Contact submission successful'}, status=status.HTTP_201_CREATED)
    print(serializer.errors)
    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

`contact.tsx`
```typescript
  
import { useState } from 'react';
import { Send } from 'lucide-react';
import { toast } from 'sonner';
import axios from 'axios';
  
const ContactForm = () => {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: '',
    message: '',
    interested: '',
    location: '',
  });
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement | HTMLSelectElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
 };

  
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setIsSubmitting(true);
    try {
      const response = await axios.post('http://127.0.0.1:8000/api/contact/submit/', formData);
      toast.success('Thank you for your message!', {
        description: 'We will get back to you shortly.',
      });
      // Reset form after successful submission
      setFormData({
        name: '',
        email: '',
        phone: '',
        message: '',
        interested: '',
        location: '',
      });
    } catch (error) {
      toast.error('Something went wrong. Please try again.');
    }
    setIsSubmitting(false);
  };
  
  return (
    <form onSubmit={handleSubmit} className="space-y-6">
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label htmlFor="name" className="block text-sm font-medium text-twistar-500 mb-1">
            Full Name
          </label>
          <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChange}
            required
            className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition"
            placeholder="John Doe"
          />
        </div>
        <div>
          <label htmlFor="email" className="block text-sm font-medium text-twistar-500 mb-1">
            Email Address
          </label>
          <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            required
            pattern='[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$'
            className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition"
            placeholder="krish@example.com"
          />
        </div>
      </div>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label htmlFor="phone" className="block text-sm font-medium text-twistar-500 mb-1">
            Phone Number
          </label>
          <input
            type="tel"
            id="phone"
            name="phone"
            value={formData.phone}
            onChange={handleChange}
            pattern='[0-9]{10}'
            className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition"
            placeholder="(123) 456-7890"
          />
        </div>
        <div>
          <label htmlFor="interested" className="block text-sm font-medium text-twistar-500 mb-1">
            I'm Interested In
          </label>
          <select
            id="interested"
            name="interested"
            value={formData.interested}
            onChange={handleChange}
            className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition bg-white"
          >
            <option value="gymnastics">Gymnastics</option>
            <option value="calisthenics">Calisthenics</option>
            <option value="strength">Strength Training</option>
            <option value="all">All Programs</option>
          </select>
        </div>
        <div>
          <label htmlFor="interested" className="block text-sm font-medium text-twistar-500 mb-1">
            Location
          </label>
          <select
            id="location"
            name="location"
            value={formData.location}
            onChange={handleChange}
            className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition bg-white"
          >
            <option value="HSRB">HSRB</option>
            <option value="Hebbal">Hebbal</option>
            <option value="Jakkur">Jakkur</option>
          </select>
        </div>
      </div>
      <div>
        <label htmlFor="message" className="block text-sm font-medium text-twistar-500 mb-1">
          Message
        </label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          rows={4}
          className="w-full px-4 py-3 rounded-lg border border-twistar-200 focus:outline-none focus:ring-2 focus:ring-twistar-accent focus:border-transparent transition"
          placeholder="Tell us about your fitness goals or any questions you have..."
        ></textarea>
      </div>
      <button
        type="submit"
        disabled={isSubmitting}
        className="btn-primary w-full py-3 px-6 flex items-center justify-center gap-2 rounded-lg text-base md:text-lg font-medium transition-all"
      >
        {isSubmitting ? (
          <>
            <span className="animate-spin rounded-full h-4 w-4 border-b-2 border-white"></span>
            Sending...
          </>
        ) : (
          <>
            Send Message <Send size={18} />
          </>
        )}
      </button>
    </form>
  );
};

export default ContactForm;
```

<h5>python manage.py runserver</h5>
Now, you can access:

- `http://127.0.0.1:8000/api/user/`
