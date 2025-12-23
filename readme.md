### Django settings.py File
The settings.py file is the central configuration file of a Django project.
It defines how the Django application behaves, including:
- Security settings
- Installed applications
- Middleware
- Database configuration
- Static and media files
- Templates
- Internationalization
- Deployment-related settings
Django loads this file when the server starts.
### What is SECRET_KEY?
SECRET_KEY is a cryptographic key used by Django for security-related operations.
**Purpose**
It is used to:
- Sign session cookies
- Generate CSRF tokens
- Hash passwords
- Create secure tokens
- Protect against tampering
```
SECRET_KEY = 'django-insecure-abc123xyz'
```
### What are the default Django apps inside it? Are there more?
Django comes with built-in applications enabled by default.
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

### What is middleware? What are different kinds of middleware? Read up a little on each security issue.
Middleware is a layer between the request and response cycle.
It processes:
- Incoming HTTP requests before they reach the view
- Outgoing HTTP responses before they are returned to the clien
**Types of Middleware**
  ```
  MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

**SecurityMiddleware**
- Adds security headers
- Enforces HTTPS
- Prevents SSL attacks
- Enables HSTS (HTTP Strict Transport Security)
  
**SessionMiddleware**
- Enables session handling
- Stores user-specific data between requests
  
**CommonMiddleware**
- URL normalization
- Handles broken links
- Adds Content-Length headers
  
**AuthenticationMiddleware**
- Associates users with requests
- Enables request.user
  
**MessageMiddleware**
- Enables flash messages (success, error, warning)
  
**Clickjacking Protection Middleware**
- Prevents site from being embedded in malicious iframes
### Django Security
Django is designed with security by default. It protects against many common web attacks automatically.
### CSRF (Cross-Site Request Forgery)
An attack where a malicious website tricks a user into performing actions on another site where they are authenticated.
**Example**
A user logged into a banking site unknowingly submits a malicious transfer request.
**Django Protection**
- Uses CSRF tokens
- Enabled via CsrfViewMiddleware
- Requires {% csrf_token %} in HTML forms
### XSS (Cross-Site Scripting)
Injection of malicious JavaScript into web pages viewed by other users.
```
<script>alert("Hacked!")</script>
```
**Django Protection**
- Automatic HTML escaping in templates
- Prevents unsafe rendering
- Safe only when explicitly marked (|safe)
### Clickjacking
Tricks users into clicking hidden elements embedded in iframes.
**Django Protection**
- XFrameOptionsMiddleware
**Adds headers:**
- X-Frame-Options: DENY
or SAMEORIGIN
### Other Important Security Middleware
**HTTPS Redirection**
- Forces all traffic over HTTPS.
**Secure Cookies**
- SESSION_COOKIE_SECURE
- CSRF_COOKIE_SECURE
**Content Security Policy (CSP)**
- Limits sources of scripts, images, styles
### WSGI 
WSGI (Web Server Gateway Interface) is a standard interface between:
- A web server (e.g., Nginx, Apache)
- A Python web application (e.g., Django)
**Why WSGI Exists**
Different servers and frameworks need a common way to communicate.
```
# wsgi.py
import os
from django.core.wsgi import get_wsgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'project.settings')
application = get_wsgi_application()
```
### Django Models File
- The models.py file defines the data structure of your application.
- Each model represents a database table, and each attribute of the model represents a column in that table.
- Django uses an Object Relational Mapper (ORM), which allows developers to interact with the database using Python code instead of SQL.
 ```
  from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
```
### What is on_delete=models.CASCADE?
on_delete=models.CASCADE defines what happens to related objects when a referenced object is deleted.
```
class Course(models.Model):
    title = models.CharField(max_length=100)

class Student(models.Model):
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
```
**What Does CASCADE Mean?**
- If a Course is deleted
- All Students linked to that course are also deleted
### A broad understanding of Fields and Validators available to you
**Fields**
Fields define what type of data can be stored in a database table column.
| Field           | What it Stores           |
| --------------- | ------------------------ |
| `CharField`     | Short text (name, title) |
| `TextField`     | Long text (description)  |
| `IntegerField`  | Numbers (age, marks)     |
| `BooleanField`  | True or False            |
| `DateField`     | Date only                |
| `DateTimeField` | Date and time            |
| `EmailField`    | Email address            |
| `URLField`      | Website link             |
| `FileField`     | Any file                 |
| `ImageField`    | Image file               |
| `ForeignKey`    | Link to another table    |
**Simple Example**
```
class Student(models.Model):
    name = models.CharField(max_length=50)
    age = models.IntegerField()
    email = models.EmailField()
```
**Validators**
Validators are checks that make sure the data is correct before saving.
| Validator           | What It Checks   |
| ------------------- | ---------------- |
| `MinValueValidator` | Minimum value    |
| `MaxValueValidator` | Maximum value    |
| `EmailValidator`    | Valid email      |
| `RegexValidator`    | Pattern matching |
| `URLValidator`      | Valid URL        |

**Simple Example with Validators**
```
from django.core.validators import MinValueValidator, MaxValueValidator

class Exam(models.Model):
    marks = models.IntegerField(
        validators=[MinValueValidator(0), MaxValueValidator(100)]
    )
```
### Understanding the difference between Python module and Python class?
**Python Module**
- A module is a file
- The file ends with .py
- It is used to keep code together
```
models.py   → module
```
**Python Class**
- A class is written inside a module
- It is a template to create objects
- It describes what something is and what it can do
```
class Student:
    pass
```
### Django ORM
- ORM stands for Object Relational Mapper
- It allows you to work with the database using Python instead of SQL
- Each model = database table
- Each object = table row
### Using ORM queries in Django Shell
**Step 1: Open Django Shell**
```
python manage.py shell
```
**Step 2: Import Your Model**
```
from app.models import Student
```

**Step 3: Common ORM Queries**
Create Data
```
Student.objects.create(name="Alice", age=20)
```

**Read Data**
```
Student.objects.all()
```
**Filter Data**
```
Student.objects.filter(age=20)
```
**Get Single Object**
```
Student.objects.get(id=1)
```
**Update Data**
```
student = Student.objects.get(id=1)
student.age = 21
student.save()
```
**Delete Data**
```
student.delete()
```
### Turning ORM to SQL in Django Shell
To see the SQL query that Django sends to the database.
**Step 1: Open Django Shell**
```
python manage.py shell
```
**Step 2: Import Model**
```
from app.models import Student
```
**Step 3: Write an ORM Query**
```
qs = Student.objects.filter(age=20)
```
**Step 4: See the SQL Query**
```
print(qs.query)
```
**Simple Example**
**ORM**
```
Student.objects.all()
```
**SQL**
```
SELECT * FROM student;
```
**See All SQL Queries**
```
from django.db import connection
print(connection.queries)
```
### What are Aggregations?
Aggregations are used to calculate values from many database records.
**Common Aggregation Functions**
| Aggregation | What it Does         |
| ----------- | -------------------- |
| `Count`     | Counts records       |
| `Sum`       | Adds values          |
| `Avg`       | Finds average        |
| `Max`       | Finds largest value  |
| `Min`       | Finds smallest value |
**Using Aggregations in Django Shell**
**Import**
```
from django.db.models import Count, Sum, Avg, Max, Min
from app.models import Student
```
**Count**
```
Student.objects.count()
```
**Sum**
```
Student.objects.aggregate(Sum('marks'))
```
**Average**
```
Student.objects.aggregate(Avg('marks'))
```
**Maximum**
```
Student.objects.aggregate(Max('marks'))
```
**Minimum**
```
Student.objects.aggregate(Min('marks'))
```
### What are Annotations?
- Annotations add a calculated value to each record in a query.
- Aggregation → gives one result
- Annotation → gives one result per object
**Simple Example**
```
class Student(models.Model):
    marks = models.IntegerField()
```
**Using Annotations**
**Step 1: Import**
```
from django.db.models import Count, Avg
from app.models import Student
```
**Example 1: Add Average Marks to Each Student**
```
Student.objects.annotate(avg_marks=Avg('marks'))
```
### What is a migration file? Why is it needed?
A migration file is a file that records changes made to models and tells Django how to update the database.
- It is auto-generated by Django
- Stored inside the migrations/ folder
- Written in Python
**Example**
  ```
  class Student(models.Model):
    name = models.CharField(max_length=50)
    age = models.IntegerField()

**Why is Migration Needed?**
To keep the database structure synchronized with Django models.
**1. Keeps Database in Sync**
Ensures database structure matches models.py
**2. No Manual SQL Needed**
Django converts model changes into SQL automatically
**3. Tracks Changes**
Keeps history of all database changes
**4. Safe Updates**
Avoids data loss and errors
**5. Works Across Environments**
Same database changes apply to development, testing, and production
### What are SQL transactions? (non ORM concept)
An SQL transaction is a group of database operations that are treated as one single unit of work.
- Either all operations succeed
- Or none of them are applied
This keeps data safe and consistent.
**Example in SQL**
  ```
BEGIN;
UPDATE account SET balance = balance - 100 WHERE id = 1;
UPDATE account SET balance = balance + 100 WHERE id = 2;
COMMIT;
| Command     | Meaning           |
| ----------- | ----------------- |
| `BEGIN`     | Start transaction |
| `COMMIT`    | Save changes      |
| `ROLLBACK`  | Undo changes      |
| `SAVEPOINT` | Set a checkpoint  |

### What are atomic transactions?
An atomic transaction means a transaction that is all or nothing.
- Either all steps succeed
- Or all steps fail and are undone

**Atomic vs Normal Operations**
  | Normal Operation           | Atomic Transaction         |
| -------------------------- | -------------------------- |
| Changes saved step by step | Changes saved together     |
| Partial updates possible   | Partial updates impossible |
| Risky                      | Safe                       |

