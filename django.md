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

Django Models are used to create tables in our database. To create a model. Go into your apps directory and then in the models.py file. we are going to create three tables for a library management system

```python
# models.py


```
