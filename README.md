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
   # Getting Started with installing Django and Django REST Framework
   pip install django djangorestframework

   # Creating the new Django Project named "Diversified":
   mkdir diversified
   CD diversified
   django-admin startproject diversified

   # Create a new app for the project
   CD diversified
   python manage.py startapp resources

   ## Configuring Django (in the setting.py add the necessary configurations below:
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


   To get started with this project, follow these steps:
