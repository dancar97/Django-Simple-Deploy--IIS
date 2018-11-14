# Django-Simple-Deploy--IIS
A simple example for deploying a basic Django app on IIS

### Notes:
+ Python version: 3.6
+ Django version: 2.1.3
+ psycopg2 version: 2.7.6
+ djangorestframework version: 3.0.9
+ requests version: 2.20.1
---
#### Important:  
-
  - These versions are needed for the correct functioning and deployment, python 3.7 presents problems with libraries used on this project and versions below 3.6 work with a different version of Django.
---

# How to make it work:

## 1. First step, installing python

Go to [this page](https://www.python.org/downloads/release/python-360/) and install python 3.6, remember that is very important to have this version, otherwise your project might not work.

![texto](https://github.com/dancar97/Django-Simple-Deploy--IIS/blob/master/1.png?raw=true)
If your current path has whitespaces you should change it with the "Custom installation" button. If that is not the case, you should check the "Add python to path" checkbox. 

If you had to change your path, you should check the environment variables to add python 3 and python3/scripts

## 2. Installing Django and other libraries

You have to install:
  + Django
  + djangorestframework
  + requests
  + psycopg2

For working versions you may want to check the first part of this readme

to install you have to write this on a command prompt:
```
pip install "your library here"
```
## 3. Building a basic Django project
To make a basic Django project you have to run the following comands:
```
django-admin startproject "Name of the project" 
```

this sentence will create a folder with some files, including a "manage.py" script.
Now we have to write:

```
cd "Name of the project"
```

this will open our new directory.
Finally, we get to start a new app, Django projects work using "apps" or functionalities using:
```
python manage.py startapp "Name of the app"
```
##### note: you should write "Name of the project" and "Name of the apps" without quotation marks.

## 4. Creating migrations

By default Django works with an SQLite database, you can change that configuration on the "settings.py" script. for more information you should check the oficial django documentation [right here](https://docs.djangoproject.com/)

To create migrations we first may want to add our app into the "settings.py" script.
Under 'INSTALLED_APPS' you should write:
```
'name of your app',
```

with single quotation marks

now we have to create a 'model'. On your app directory, you may find a "models.py" script. there you can modify the structure of your data.

For example, on my test application, I wrote the following code:
```
from django.db import models
from django.template.defaultfilters import slugify
from django.contrib.auth.models import User

class User(models.Model):
	name = models.CharField(max_length=50)
	id_User = models.AutoField(primary_key=True)

```

This will create a simple 'User' with a name and an ID

Now we have to migrate:

Use:

```
python manage.py makemigrations
python manage.py migrate
```
This comands create space on the DB for the default django admin information and the model you just created.

Now we have to make a superuser acount for the default admin:
```
python manage.py createsuperuser
```

This will get you important credentials to manage your databases via 'Admin' control, remember your credentials for later.

## 5. Run a test

You should be able to run a simple test by now.
run:
```
python manage.py runserver
```
Now you have to paste:
> http://127.0.0.1:8000/

on a browser to open the new django project. If everything is working, you should see this:

![texto](https://github.com/dancar97/Django-Simple-Deploy--IIS/blob/master/2.png?raw=true)
* Note: versions of django may vary this result

Now we have our app runing, try to write:

> http://127.0.0.1:8000/admin

## 6. IIS conection




