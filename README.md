# Django-Simple-Deploy--IIS
A simple example for deploying a basic Django app over IIS

### Notes:
+ Python version: 3.6
+ Django version: 2.1.3
+ wfastcgi version : 3.0.0
+ psycopg2 version: 2.7.6 (If you want to run on postgresql)
+ djangorestframework version: 3.0.9 (If you want to run a rest service)
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
  + requests

For working versions you may want to check the first part of this readme

to install you have to write this on a command prompt:
```
pip install "your library here"
```
## 3. Building a basic Django project
To make a basic Django project you have to run the following commands:
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

By default Django works with an SQLite database, you can change that configuration on the "settings.py" script. for more information you should check the official django documentation [right here](https://docs.djangoproject.com/)

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
This commands create space on the DB for the default django admin information and the model you just created.

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

## 6. IIS connection

We have to install wfastcgi via pip, use:
```
pip install wfastcgi
```
now run this via console:
 ```
 wfastcgi-disable
 wfastcgi-enable
 ```
 
Pay attention to the output, there should be something like this:
>"your python path"|"your wfastcgi path"

Go to your wfastcgi path and copy "wfastcgi.py" and paste it on your django project folder, next to manage.py

Go to IIS manager and create your new Website, point the path to your django project folder

We need to create a web.config file with this code inside:
  ```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<system.webServer>
		<handlers>
  
			<add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="c:\python3\python.exe|&quot;C:\path_to_Django_project\wfastcgi.py&quot;" resourceType="Unspecified" requireAccess="Script" />
		</handlers>\
	</system.webServer> 

	<appSettings>

		<add key="WSGI_HANDLER" value="name_of_app.wsgi.application" />
		<add key="PYTHONPATH" value="C:\Path\toProject" />

		

		<add key="DJANGO_SETTINGS_MODULE" value="name_of_app.settings" />
	</appSettings>
</configuration>
```

On 'scriptProcessor' you should change the first part to: 
```
your python path|"your wfastcgi.py(in the project folder, not the python folder)"
```

be careful with the quotation marks at the second part, you should leave them

also, change "name_of_app" with the name you gave on the "python manage.py startapp" to your app

* note: notice the 
```
&quot;
```
on the scriptProcessor part, you should only leave double quotation marks on the wfastcgi.py part, not on your python path(unless it has whitespaces, wich is a big problem during this installation)

Now we have to set our project folder to a pool, we are going to do this by right clicking our django project parent directory and we are going to select properties.

Then, on security tab you may find an 'Edit' button. Click on it and select "add".

On name write:
```
IIS AppPool/"your_pool"
```
click on check names and click the 'Ok' button.
click 'apply' and 'ok' buttons untill you are done with the properties window.

* note: if you have problems of security later, you may want to re do this whole part for your python folder

now its time to generate a statics folder for the project. Open a console that points to your "manage.py" again and write:

```
python manage.py collectstatic
```

this will create a new folder called "static", open that folder and create a new "web.config" file with this inside:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<system.webServer>
        <!-- Overrides the FastCGI handler to let IIS serve the static files -->
		<handlers>
            <clear/>
			<add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="File" requireAccess="Read" />              
		</handlers>
	</system.webServer>
</configuration>
```

Now open the IIS and on your server, search for "Configurations editor" under the management tab. Click on it and open the "system/webServer.handlers and unlock it by clicking "unlock" on the right side panel.

---
* Note: Next step is optional:

##### If you want to add your static folder to the project, go to IIS and right click your Website. Select the add virtual directory and name it "static" then you just have to point at the static folder that was created a few steps before in the django project.
---


For our final step, we have to select our webpage on the IIS, search for "Handler Mappings" under IIS tab.
Click on it and add a new Managed Handler.

Name your module FastCgiModule.

On executable paste the same path that was on your Web.config under scriptprocessor.

>PythonPath|"project's wfastcgi.py" 

now just click 'ok' and everything should be working.


