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
>pip install "your library here"

## 3. Building a basic Django project

 


