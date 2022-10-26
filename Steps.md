# Windows

$ cd f:\#B\WSV_PRO\
$ mkdir bookstore
$ cd ch4-bookstore
$ python -m venv .venv
$ Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
$ .venv\Scripts\Activate.ps1
(.venv) $ python -m pip install django~=4.1.0 psycopg2-binary==2.9.3

(.venv) $ django-admin startproject project .
(.venv) $ py manage.py runserver
Check in broweser
(.venv) $ ctrl+c
(.venv) $ pip freeze > requirements.txt
(.venv) $ code .
Docker:
(.venv) $ deactivate
$
Docker should already be installed and the desktop app running
Create these files:
Dockerfile
docker-compose.yml
.gitignore
.dockerignore

FIll in Dockerfile, docker-compose.yml, .gitignore & .dockerignore as done.

Create base image
$ docker-compose up -d --build

Refresh the browser with 127.0.0.1:8000
It should be the same friendly Django welcome page albeit now running inside of Docker.

---

PostgreSQL
Edit project/settings.py to add postdressql database

django_project/settings.py

DATABASES = {
"default": {
"ENGINE": "django.db.backends.postgresql",
"NAME": "postgres",
"USER": "postgres",
"PASSWORD": "postgres",
"HOST": "db",
"PORT": 5432,
}
}

Refresh the browser to confirm that everything is running ok

Custom User Model

1. create accounts app
2. Create a CustomUser model
3. Update django_project/settings.py
4. Customize UserCreationForm and UserChangeForm
5. Add the custom user model to admin.py

Doing this we will be running our commands within Docker itself.
So, normal django commands will be precesed with [docker-compose exec web]
$ docker-compose exec web python manage.py startapp accounts

Create a new CustomUser model which extends AbstractUser.

Then edit settings.py adding "accounts.apps.AccountsConfig", TO INSTALLED_APPS
and add this [AUTH_USER_MODEL = "accounts.CustomUser"] following INSTALLED_APPS

Now we can create a migrations for the changes.
$ docker-compose exec web python manage.py makemigrations accounts
Then
$ docker-compose exec web python manage.py migrate

Create accounts/forms.py
Fill as done

Custom User Admin

Superuser
$ docker-compose exec web python manage.py createsuperuser
admin
admin@example.com
testpass123
testpass123

Tests
Unit Tests
accounts/tests.py Fill in
$ docker-compose exec web python manage.py test

Git
$ git init
$ git status
$ git add .
$ git commit -m "ch4-bookstore"

================================================================

Chapter 5: Pages App

$ docker-compose exec web python manage.py startapp pages

add to settings.py
"pages.apps.PagesConfig", # new [INSTALLED_APPS]
and
"DIRS": [BASE_DIR / "templates"], # new [TEMPLATES]

$ mkdir templates
Create files templates/\_base.html & home.html

URLs and Views

You have to update the image.. so.....

$ docker-compose down

$ docker-compose up -d

=====================================================

Chapter 6: User Registration

Auth URLs and Views
To use Django’s built-in auth app we must explicitly add it to our config/urls.py file. The easiest
approach is to use accounts/ as the prefix since that is commonly used in the Django community.
Check modified project/url.py

1. Update templates/home.html to display choices

django/contrib/auth/urls.py (source code)

from django.contrib.auth import views
from django.urls import path

urlpatterns = [
path('login/', views.LoginView.as_view(), name='login'),
path('logout/', views.LogoutView.as_view(), name='logout'),
path('password_change/', views.PasswordChangeView.as_view(),
name='password_change'),
path('password_change/done/', views.PasswordChangeDoneView.as_view(),
name='password_change_done'),
path('password_reset/', views.PasswordResetView.as_view(),
name='password_reset'),
path('password_reset/done/', views.PasswordResetDoneView.as_view(),
name='password_reset_done'),
path('reset/<uidb64>/<token>/', views.PasswordResetConfirmView.as_view(),
name='password_reset_confirm'),
path('reset/done/', views.PasswordResetCompleteView.as_view(),
name='password_reset_complete'),
]

Sign Up (register)
• create an app-level accounts/urls.py file
• update the project-level django_project/urls.py to point to the accounts app
• add a view called SignupPageView
• create a signup.html template file
• update home.html to display the sign up page

============================

Chapter 7: Static Assets
Local Development
$ mkdir static
$ mkdir static/css
$ mkdir static/js
$ mkdir static/images

JavaScript

Production
(.venv) $ docker-compose exec web python manage.py collectstatic

STATICFILES_FINDERS !!!!!

Bootstrap
Added latest bootstrap 5.2 & css\navbar-top-fixed.css

KEEP THIS SETUP FOR HOMEPAGE FOR FUTURE USE

About Page

Important
Update the image:
$ docker-compose down
$ docker-compose up -d

About Page Tests

Tests

Django Crispy Forms
Add crispy to requirements.txt

Update the image:
$ docker-compose down
$ docker-compose up -d --build
