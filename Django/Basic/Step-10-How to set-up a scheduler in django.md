### 1. Create a directory in the Created app folder
<p>So i assume that you have created the app to schedule a task that need to be executed automatically we first have to create a directory inside the app with directory named as <span style="color:green;">management/commands/</span> inside this directory create the .py file which contains the code that perform the automated scheduled task.  </p>

```python run_task.py
from django.core.management.base import BaseCommand
import time

class Command(BaseCommand):
		help = "Run a simple task"

		def handle(self, *args, **options):
			while True:
					print("hello Congratulation this is a schedule task. ")
					time.sleep(10)
			#so this lines of code schedule an automated task to print the msg for every 10 seconds	
```


### 2.To check whether you code works or not
<p> Just go to manage.py file directory and in the terminal run the command <span style="color:green;">python3 manage.py run_task</span> assuming that the .py file created at <span style="color:green;">1</span> as run_task.py</p>


