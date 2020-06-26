# Django + React Cheatsheet
A simple guide on how to serve a React app with Django.


## Content
- [Setting up Django](#setting-up-django)
- [Editing Django settings](#editing-django-settings)
- [Setting up a dummy Django view to test our setup](#setting-up-a-dummy-django-view-to-test-our-setup)
- [Setting up React](#setting-up-react)
- [Testing our setup](#testing-our-setup)

**tl;dr: A React app is just a bunch of static files. Serving it with Django boils down to telling Django where to find said static files and rendering the React's app *index.html*.**

## Setting up Django
- `mkdir project`
- `cd project`
- Create [virtualenv](https://virtualenv.pypa.io/): `virtualenv venv`
- Activate virtualenv: `source venv/bin/activate`
- Install Django: `pip install django`
- Initialize Django project: `django-admin startproject backend`

## Editing Django settings
- Open the backend/backend/settings.py file and make the following changes:
```
TEMPLATES = [
    ...

    'DIRS': [os.path.join(BASE_DIR, '..', 'frontend', 'build')]

    ...
]
```
This basically tells Django "look for React's html file in project/frontend/build."

Under STATIC_URL, add the following:
```
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, '..', 'frontend', 'build', 'static'),
)
```
This tells Django "look for React's static files in project/frontend/build/static."

## Setting up a dummy Django view to test our setup

- Create a file backend/backend/views.py with the following content:
```
from django.shortcuts import render
       
def index(request):
    return render(request, 'react.html')
```
       
- Edit backend/backend/urls.py with the following content:
```
from django.urls import path
       
from .views import index
       
urlpatterns = [
    path('', index),
]
```


## Setting up React
- Go to the project base directory (i.e. the directory where the *venv* and *backend* folders are).
- Create the React boilerplate with: `npx create-react-app frontend`
- Edit frontend/package.json. Change the line

`"build": "react-scripts build”,`
       
to
       
`"build": "react-scripts build && mv build/index.html build/react.html",`
       
This is to ensure React’s html file doesn’t clash with your own *index.html* file, if you have/plan to have one.

- Build the example React app with: `npm run build`

## Testing our setup

- Go to project/backend and execute: `python manage.py runserver`

If everything went ok, you should see create-react-app's example app in http://127.0.0.1:8000/

That's it.