# Diversified Semester Project

## Overview

Hello and good-afternoon, this is my **Neurodivergent Awareness Project called Diversified**!

## Table of Contents

- [Overview](#overview)
- [Project Summary](#Project-Summary)
   - [Installation](#Installation)
   - [Getting Started](#Getting-Started)
- [User Stories](#User-Stories)
   - [Acceptance Criteria](#Acceptance-Criteria)
   - [Mis-User Stories](#Mis-User-Stories)
   - [Mitigating Criteria](#Mitigating-Criteria)
- [Diagrams](#Diagrams)
   - [Mockup](#Mockup)
   - [C4 Diagrams](#C4)
- [Prototype](#Prototype)
- [License](#license)
- [Contact](#contact)
- [Citations](#Citations)

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
    'django_filters',
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
This serializer is to convert model instances to JSON and to validate incoming user data
   ```bash
   from rest_framework import serializers
   from .models import Resource

   class ResourceSerializer(serializers.ModelSerializer):
    class Meta:
        model = Resource
        fields = '__all__'

11. Create a serializer in each app: ```community/serializers.py```
This serializer is for posts
    ```bash
    from rest_framework import serializers
    from .models import Post

    class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = '__all__'
    
13. Create the views and route for the API: ```recources/views.py```
Using Django Rest Framework ```Viewset``` to define the behavior for CRUD operations
    ```bash
    from rest_framework import viewsets
    from rest_framework.filters import SearchFilter
    from django_filters.rest_framework import DjangoFilterBackend
    from .models import Resource
    from .serializers import ResourceSerializer

    class ResourceViewSet(viewsets.ModelViewSet):
        queryset = Resource.objects.all()
        serializer_class = ResourceSerializer
        filter_backends = [DjangoFilterBackend, SearchFilter]
        filterset_fields = ['published_date']  # Allow filtering by published date
        search_fields = ['title', 'description']  # Allow searching by title and description
    
15. Create the viewset for community posts
    ```bash
    from rest_framework import viewsets
    from rest_framework.filters import SearchFilter
    from django_filters.rest_framework import DjangoFilterBackend
    from .models import Post
    from .serializers import PostSerializer

    class PostViewSet(viewsets.ModelViewSet):
        queryset = Post.objects.all()
        serializer_class = PostSerializer
        filter_backends = [DjangoFilterBackend, SearchFilter]
        filterset_fields = ['created_at']  # Allow filtering by creation date
        search_fields = ['title', 'content']  # Allow searching by title and content
    
17. Update the URL's in ```diversified/urls.py```
The author will demonstrate how to update the urls to add the necessary routers!
    ```bash
    from django.contrib import admin
    from django.urls import path, include
    from rest_framework.routers import DefaultRouter
    from resources.views import ResourceViewSet
    from community.views import PostViewSet

    router = DefaultRouter()
    router.register(r'resources', ResourceViewSet)
    router.register(r'community', PostViewSet)

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/', include(router.urls)),
    ]
    
19. We will now be dockerizing the application
Create a ```dockerfile``` at the root of the project directory and add the following to it:
    ```bash
    # Use official Python image from Docker Hub
    FROM python:3.11-slim

    # Set environment variables to avoid Python buffering
    ENV PYTHONUNBUFFERED 1

    # Set the working directory
    WORKDIR /app

    # Install dependencies
    COPY requirements.txt /app/
    RUN pip install --no-cache-dir -r requirements.txt

    # Copy project files to container
    COPY . /app/

    # Expose port
    EXPOSE 8000

    # Run the Django development server
    CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
    
20. Create a ```docker-compose.yml```:
    ```bash
    services:
      web:
       build: .
       command: python manage.py runserver 0.0.0.0:8000
       volumes:
         - .:/app
       ports:
         - "8000:8000"
       depends_on:
         - db

      db:
       image: postgres:13
       volumes:
          - postgres_data:/var/lib/postgresql/data
       environment:
          POSTGRES_DB: neurodiverse_db
          POSTGRES_USER: user
          POSTGRES_PASSWORD: password
    volumes:
      postgres_data:
    
21. Create the ```requirements.txt``` file:
    ```bash
    Django>=4.0,<5.0
    djangorestframework>=3.13,<4.0
    psycopg2>=2.9,<3.0  # For PostgreSQL
    gunicorn>=20.1,<21.0
    
22. Using GitHub for Version Control
    ```bash
    git init
    git remote add origin <your-github-repository-url>
    git add .
    git commit -m "Initial commit"
    git push -u origin main

#### Getting-Started
1. After this we can finally do our docker command to stand the app up to be able to use docker!
    ```bash
    docker compose up --build
    docker-compose run django bash
    python manage.py migrate
    python manage.py makemigrations
    python manage.py createsuperuser
    docker-compose up

### User-Stories

1. User Story:
   ```bash
   As a person with autism, I want to be able to submit and post certain things that I am passionate about others learning about so that I can not only educate others but also to spread awareness and a safe space for others with neuro-divergent conditions.

2. User Story:
   ```bash
   As a neuro-divergent individual I want to be able to view the communities posts that I am interested in so that I may be able to comment and share/spread my expereinces as well
   
3. User Story:
   ```bash
   As an **Administrator** I want to be able to not only have the right to take on and off posts that are harmful and cross the community guidelines as well as ensure that others aren't abusing policy!

4. User Story:
   ```bash
   As an **Administrator** I want to be able to pull all comments and provide statistics to interested buyers so that they can see how many people view the site as well as what they are doing while on the site to better provide a solid starting point for possibly a futher development of the web api

#### Acceptance-Criteria

Requirements for the user stories to be realized!

#### Mis-User Stories

Malicious user stories

#### Mitigating Criteria

Requirmenets you put in a system to prevent mis-user stories

### Diagrams

#### Mockup

#### C4

[Good Luck]

<!-- LICENSE -->
### License

Distributed under the MIT License. See `LICENSE.txt` below for more information.

MIT License

Copyright (c) 2024 Dominic Lanzante

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


<!-- CONTACT -->
### Contact

Dominic Lanzante - [@my_instagram](https://www.instagram.com/dlanzante2018/) - dlanzante2018

Dlanzante@unomaha.edu | 402.554.2648 | University of Nebraska Omaha
University of Nebraska Omaha, 6001 Dodge Street, Omaha, NE, 68182

Project Link: [https://github.com/DLanzante/](https://github.com/DLanzante/) - Github Main
