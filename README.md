# Diversified Semester Project

## Overview

Hello and good-afternoon, this is my **Neurodivergent Awareness Project called Diversified**!

## Table of Contents

- [Overview](#overview)
- [Project Summary](#Project-Summary)
- [Features](#features)
- [Getting Started](#getting-started)
- [Contributing](#contributing)
- [Resources](#resources)
- [License](#license)
- [Contact](#contact)

### Project-Summary

This project is dedicated to spreading awareness about neurodivergent conditions, such as **Autism**, **ADHD**, and other related diagnoses. The authors goal is to provide resources, educational content, and a collaborative space to increase understanding and reduce stigma surrounding neurodiversity. This is going to be different then Reddit because with this web app it is gpong to be developed and made for those who are neurodiverse!

This project has several core objectives:

- **Raise Awareness**: Share information and raise awareness about neurodivergent conditions such as Autism, ADHD, and others.
- **Encourage Understanding**: Foster empathy and reduce misconceptions surrounding neurodivergence.
- **Provide Resources**: Curate educational materials, tools, and support networks for neurodivergent individuals
- **Create a Collaborative Community**: Offer a space for contributors to share resources, experiences, and ideas.

#### Installation
We are going to be going over some of the most basic instructions of how to install all necessary repositories as well as how to get the api started and how to get started using my Diversified API!
   ```bash
   # 1.Getting Started with installing Django and Django REST Framework
   pip install django djangorestframework

   # 2.Creating the new Django Project named "Diversified":
   mkdir diversified
   CD diversified
   django-admin startproject diversified

   # 3.Create a new app for the project
   CD diversified
   python manage.py startapp resources

   # 4.Configuring Django (in the setting.py add the necessary configurations below:
   INSTALLED_APPS = [
    ...
    'rest_framework',  # Add DRF
    'resources',  # Your app
]

# Optionally, set up CORS headers if your frontend is on a different domain
INSTALLED_APPS += ['corsheaders']

MIDDLEWARE = [
    ...
    'corsheaders.middleware.CorsMiddleware',
    ...
]

# CORS (Cross-Origin Resource Sharing) setup for API usage
CORS_ORIGIN_ALLOW_ALL = True  # Be cautious with this in production

   #5.Creating personal models:
   from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.CharField(max_length=100)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    category = models.CharField(max_length=100, choices=[('Autism', 'Autism'), ('ADHD', 'ADHD'), ('General', 'General')])

    def __str__(self):
        return self.title

class Resource(models.Model):
    name = models.CharField(max_length=200)
    description = models.TextField()
    url = models.URLField()
    category = models.CharField(max_length=100, choices=[('Support', 'Support'), ('Education', 'Education'), ('Research', 'Research')])

    def __str__(self):
        return self.name
   #6.Creating my own Serializers.py file:
   from rest_framework import serializers
from .models import Article, Resource

class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = ['id', 'title', 'content', 'author', 'created_at', 'updated_at', 'category']

class ResourceSerializer(serializers.ModelSerializer):
    class Meta:
        model = Resource
        fields = ['id', 'name', 'description', 'url', 'category']
```

