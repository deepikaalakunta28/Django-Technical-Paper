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
**Explanation of Each**
1.django.contrib.admin
- Provides Django’s admin interface.
2.django.contrib.auth
- Handles authentication and authorization (users, groups, permissions).
3.django.contrib.contenttypes
- Enables generic relationships between models.
4.django.contrib.sessions
- Manages session data across requests.
5.django.contrib.messages
- Provides a messaging framework (e.g., success/error messages).
6.django.contrib.staticfiles
- Manages static files like CSS, JS, images.
**Are There More Apps?**
  Yes. Django also provides optional apps such as:
- django.contrib.sites
- django.contrib.humanize
- django.contrib.sitemaps
You can also create custom apps or install third-party apps.
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

