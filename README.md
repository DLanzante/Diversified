# Diversified Semester Project

## Overview

Hello and good-afternoon, this is my **Neurodivergent Awareness Project called Diversified**!

## Table of Contents

- [Overview](#overview)
- [Project Summary](#Project-Summary)
- [Installation](#Installation)
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
1. Clone the repository   
   ```bash
   git clone https://gtihub.com/DLanzante/Diversified

2. Change the Directory
   ```bash
   cd diversify
   
3. Run the container and view what it has to offer
   ```bash
   docker compose build && docker compose up
   localhost:8000 or your_port
   
4. Add apps and we will break them up into ```Resources``` and ```Community```
   ```bash
   python manage.py startapp resources
   python manage.py startapp community
   
5. Update our Settings ```diversified/settings.py``` and add the installed apps and django rest framework settings
   ```bash
   INSTALLED_APPS = [
    # Default apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # These are my apps that I want
    'rest_framework',  # For DRF
    'resources',       # Custom app for resources
    'community',       # Custom app for community posts
   ]
   
6. Configure our rest framework settings for API-wide configurations
   ```bash
   REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly'
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.JWTAuthentication',  # Optional: JWT Auth
    ]
   }
   
7. Create API models in each app: ```resources/models.py```
   ```bash
   from django.db import models

   class Resource(models.Model):
    title = models.CharField(max_length=200)
    description = models.TextField()
    url = models.URLField(max_length=500, null=True, blank=True)
    published_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
   
8. Create API models in each app: ```community/models.api```
   ```bash
   from django.db import models
   from django.contrib.auth.models import User

   class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title

9. Create serializers in each app: ```respources/serializers.py```
10. 
11. 
