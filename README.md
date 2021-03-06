# Django 2.0+ project template

This is a personalized fork of [Django 2.0+ project template](https://jpadilla.github.io/django-project-template/) 
with all the basic requirements as used in my projects. The original template met my basic needs but I had to do a 
lot of modifications before actually starting development. Because all my changes where personal preferences I 
preferred to create a customized version of the project. Just like the original project, this template make the least 
mount of assumptions possible while still trying provide a useful setup. Also the base project template has 
configurations to support [heroku](https://heroku.com/) deployment and since I use [heroku](https://heroku.com/) most of
 the time I will support it in this template too. 

## Features

- [Python](https://www.python.org) 3
- [Django](https://www.djangoproject.com/) 2.0+
- Based on [Pipenv](https://github.com/kennethreitz/pipenv) - the officially recommended Python packaging tool from 
[Python.org](https://www.python.org).
- [Pip](https://pypi.org/project/pip/) and [virtulenv](https://virtualenv.pypa.io/en/latest/) compatible setup
procedure.
- Development, Staging and Production settings with 
[django-configurations](https://django-configurations.readthedocs.org).
- Get value insight and debug information while on Development with 
[django-debug-toolbar](https://django-debug-toolbar.readthedocs.org).
- Collection of custom extensions with [django-extensions](http://django-extensions.readthedocs.org).
- HTTPS and other security related settings on Staging and Production.
- Procfile for running gunicorn with New Relic's Python agent.
- PostgreSQL database support with psycopg2.
- Externalized logging configuration based on 
[Good logging practice in Python](https://fangpenlin.com/posts/2012/08/26/good-logging-practice-in-python/)

## How to install
This project template is intended to use [Pipenv](https://github.com/kennethreitz/pipenv), also it is not a requirement.
I see some people in the community who still prefer using [pip](https://pypi.org/project/pip/) +
[virtualenv](https://virtualenv.pypa.io/en/latest/). This is why I included both setup procedures.

### using pipenv

```bash
pip3 install pipenv
mkdir project_name
cd project_name
pipenv --python <version>
pipenv install django
pipenv run django-admin.py startproject \
  --template=https://github.com/mohsen-mahmoodi/django2-project-template/archive/master.zip \
  --name=Procfile \
  --extension=py,md,env,yaml \
  project_name .
pipenv install -r requirements/common.txt
pipenv install -r requirements/dev.txt --dev
rm -rf requirements
cp example.env .env
cp example.logging.yaml logging.yaml
```

### using pip and virtualenv

#### Install virtualenv and create project directory

```bash
pip install virtualenv
mkdir project_dir
cd project_dir
virtualenv -p <python-interpreter> .env
source .env/bin/activate
```

#### Install django and create the project

```bash
mkdir project_name
cd project_name
pip install django
django-admin.py startproject \
  --template=https://github.com/mohsen-mahmoodi/django2-project-template/archive/master.zip \
  --name=Procfile \
  --extension=py,md,env,yaml \
  project_name .
pip install -r requirements/common.txt
pip install -r requirements/dev.txt
cp example.env .env
cp example.logging.yaml logging.yaml
export $(grep -v '^#' .env | xargs)
```

## Environment variables

These are common between environments. The `ENVIRONMENT` variable loads the correct settings, possible values are: `DEVELOPMENT`, `STAGING`, `PRODUCTION`.

```dotenv
ENVIRONMENT=DEVELOPMENT
DJANGO_SECRET_KEY=dont-tell-eve
DJANGO_DEBUG=yes
...
```

These settings(and their default values) are only used on staging and production environments.

```dotenv
DJANGO_SESSION_COOKIE_SECURE=yes
DJANGO_SECURE_BROWSER_XSS_FILTER=yes
DJANGO_SECURE_CONTENT_TYPE_NOSNIFF=yes
DJANGO_SECURE_HSTS_INCLUDE_SUBDOMAINS=yes
DJANGO_SECURE_HSTS_SECONDS=31536000
DJANGO_SECURE_REDIRECT_EXEMPT=
DJANGO_SECURE_SSL_HOST=
DJANGO_SECURE_SSL_REDIRECT=yes
DJANGO_SECURE_PROXY_SSL_HEADER=HTTP_X_FORWARDED_PROTO,https
```

### Reloading environment variables 

After updating or adding new environment variables, make sure to load the variables and reload the project:

#### Using `pipenv run`

Nothing required, the variables gets loaded automatically by the pipenv.

#### Using `pipenv shell`

You should exit the current pipenv shell by typing `exit` and reload the session again by running `pipenv shell` and 
then run the development server again.

#### Using `pip` and `virtualenv`

just run the command `export $(grep -v '^#' .env | xargs)` again and restart the development server.  

## Deployment

It is possible to deploy to Heroku or to your own server.

### Heroku

```bash
heroku create
heroku addons:add heroku-postgresql:hobby-dev
heroku pg:promote DATABASE_URL
heroku config:set ENVIRONMENT=PRODUCTION
heroku config:set DJANGO_ALLOWED_HOSTS='[app-name].herokuapp.com'
heroku config:set DJANGO_SECRET_KEY=`./manage.py generate_secret_key`
git push heroku master
heroku run python manage.py createsuperuser
```

## License

The MIT License (MIT)

Copyright (c) 2019 Mohsen Mahmoodi

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
