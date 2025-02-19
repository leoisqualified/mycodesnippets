# Learning Django

In this md we are looking at how django works. To create a django project
we start out as follows

## Create a Virtual Environment

```python
python -m venv <venv name>
```

## Install Django

```python
pip install django
```

## Create a project

```python
django-admin startproject <project name>
```

## Create an app

An app is a web application that has a specific meaning in your project, like a home page, a contact form, or a members database. Think of it as components. We can create a django app like so

```python
python manage.py startapp <app name>
```

After creating an app, you need to add it in the settings.py

```python
INSTALLED_APPS = [
    #some other installed apps here,
    '<your installed app>'
```

## Django Models

Django Models are used to create tables in our database. To create a model. Go into your apps directory and then in the models.py file.
we are going to create three tables for a library management system

```python
# models.py

from django.db import models
from django.contrib.auth.models import User

# Create your models here.
class Books(models.Model):
    title = models.CharField(max_length=200)
    author = models.CharField(max_length=100)
    isbn = models.CharField(max_length=13, unique=True)
    category = models.CharField(max_length=100)
    total_copies = models.IntegerField()
    available_copies = models.IntegerField()

    def __str__(self):
        return self.title


#  Member Table
class Member(models.Model):
    name = models.OneToOneField(User, max_length=100, on_delete=models.CASCADE)
    phone = models.CharField(max_length=15)
    membership_date = models.DateField(auto_now_add=True)

    def __str__(self):
        return self.name

# Borrowing
class BorrowingRecord(models.Model):
    book = models.ForeignKey(Books, on_delete=models.CASCADE)
    member = models.ForeignKey(Member, on_delete=models.CASCADE)
    borrow_date = models.DateField(auto_now_add=True)
    return_date = models.DateField(null=True, blank=True)
    due_date = models.DateField()

    def __str__(self):
        return f"{self.member.user.username} borrowed {self.book.title}"
```

Next to ensure the models are actually registered in our site.
We need to add it to the admin.py file like so. This ensures that we can access our database on the admin
site (at least i think). Do it like so

```python
from django.contrib import admin
from .models import Books, Member, BorrowingRecord

# Register your models here.
admin.site.register(Books)
admin.site.register(Member)
admin.site.register(BorrowingRecord)
```

## Create The SuperUser

Most Web Application have a Admin. Django also provides this in all our web app
We can create an admin like so

```python
python manage.py createsuperuser
```

Fill the required information after. You can visit the admin dashboard
Access the Admin Panel:
Run the server: python manage.py runserver.
Visit http://127.0.0.1:8000/admin and log in with your superuser credentials.

## View And Templates

To Begin with Views and Templates in django. We first need to define the urls paths
that will map to the respective views. We can configure this in our urls.py

```python

```
