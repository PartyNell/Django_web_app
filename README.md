# Django_web_app

# Virtual environment 
A virtual environment allows the developer to install package only in the environment and not on the computer. The virtual environment have to be activate each time we work on the project.

## Create a virtual environment
1. install python : https://www.geeksforgeeks.org/how-to-install-python-on-linux/
2. install python venv :
```
  sudo apt install python3.12-venv
```
3. create the virtual environment
```
  python -m venv env
```
A folder env is created in the repository

## Activate a virtual environment 
```
  source env/bin/activate
```

# Django
## Install Django
1. install the Django packages
```
  pip install django
```
2. save the dependancies in a requierementS.txt file
```
  pip freeze > requirements.txt
```

## Django project

### Start a project
```
  django-admin stratproject project_name directory
```

### Launch the development server
```
  python manage.py runserver
```
To consult the server go to : http://127.0.0.1:8000/

WARNING: the develepment serevr should not be used for production.

### Settings
The project can be configure using the file *project*/settings.

- **TIME_ZONE**: timezone of the project (France: 'Europe/Paris')
- **INSTALLED_APP**: django application deployed on the project
  - *by default:* admin, auth, contenttypes, sessions, messages and staticfiles
- **LANGUAGE_CODE**: project language

#### Database
By default Django use SQLite database. This one does not requiere to run another server. 
Nevertheless, SQLite could not be deployed on a product because of scalability. For that PostgreSQL, MySQL or Oracle are better.  

**Set up a database**:
1. Install the correct Python packages:
   - **PostgreSQL**: psycopg or psycopg2
   - **MySQL**: mysqlclient
   - **Oracle**: oracledb
2. Modify the DATABASES dictonary in the settings file
   - **ENGINE**:
     - **SQLite**: django.db.backends.sqlite3
     - **PostgreSQL**: django.db.backends.postgresql
     - **MySQL**: django.db.backends.mysql
     - **Oracle**: django.db.backends.oracle
   - **NAME**: name of the database
     - for **SQLite** this is the full path of the database file

   - **USER**
   - **PASSWORD**
   - **HOST**
   - **PORT**

**Documentation**: https://docs.djangoproject.com/en/5.1/topics/install/#database-installation

## Django application
In Django an application is a sub-section of the project

### Create an application

```
  python manage.py startapp application_name
```

## Views

A **view** is web page that serve a specific function with a specific template.

### Add a view 
1. Add a view function in the file *application*/views.py

```
from Django.http import HttpTResponse

def view_name(request[, arguments]):
  return   HttpResponse("html modele")
```

The URL provides the arguments. In fact, the URLConf that map an URL with a view accept keyword argument using `̀``<converter:argument_value>`̀``. Then, when a URL match with the path, then the value in the URL is given as an argument to the view.

1. add thelink between the URL and the view in *application*/urls.py (create the file urls.py)
```
from . import views

urlpatterns = [
  ...
  path('route', views.view_name, name="url_name"),
]
```

3. add the application.urls in the global URLconf in the project in *project*/urls.py
```
  from django.urls import include, path

  urlpatterns = [
      path("URL_name/", include("application.urls")),
      path("admin/", admin.site.urls),
  ]
```

The **include** function reference other URLconf.

The **path** function takes two argument a route and a view. Then the the url given by the route will display the associated view.  


## Template




## Django database

### Create a database
```
  python manage.py migrate
```
The migrate command create the necessary tables for the applications in INSTALLED_APPS (in settings.py).

WARNING: if the database already exist this step is not necessary. But access should be granted for the Django application.

### Create a model
A model is the definitive definition of the data. It is defined by a class in the models file of an *application*. It is a subclass of models.Model class. Each field of the database is define by a class variable with the specification models.*FieldType*.

```
from Djando.db import models

class model(models.Model):
  field=models.FieldType(arguments)
  foreignKey=models.ForeignKey(ForeignClass, on_delete="models.delete_mode")
```
Field:
  - **name**: name of the class variable. It is used as column name in the database
  - **type**: models.FieldType
  - **arguments**:
    - *verbose_name*: humanreadable name
    - name
    - primary_key
    - max_length
    - unique
    - blank
    - null
    - db_index
    - rel
    - *default*: default value given if nothing is set
    - editable
    - serialize
    - unique_for_date
    - unique_for_month
    - unique_for_year
    - choices
    - help_text   
    - db_column
    - db_tablespace
    - auto_created
    - validators   
    - error_messages
    - db_comment
    - db_default
    - *CharField/max_length*: maximum number of character

Relationship (between tables):
   -  *ForeignKey*: a single element of the foreign table is linked
   -  ManyToOne
   -  ManyToMany
   -  OneToOne


**Field Types**: https://docs.djangoproject.com/en/5.1/ref/models/fields/#model-field-types

### Methods
A **methods** are designed to apply a specific behaviour on an instance of a model.

#### __str__
The __str__ method return a human-readable description of a model's instance. But it is also called by the admin site to display an object or as the value inserted into a template. 

```
class model(models.Model):
  ...
  def __str__(sefl):
    return object_representation
```

**Documentation**: https://docs.djangoproject.com/en/5.1/ref/models/instances/#django.db.models.Model.__str__


### Activate a model
Activate a model means:
- create a database schema
- create a Python database-access API

1. Add the model's application in the INSTALLED_APPS (*project*/settings.py)
```
INSTALLED_APPS = [
  "application.apps.ApplicationConfig",
  ...
]
```
The **ApplicationConfig** class is in the apps file of the application folder.

2. Create migrations
```
  python manage.py makemigrations application
```
**makemigrations** create a migration file that contains the commands that should be done to apply the changes done on the models.

To see the SQL commands associated: ```python manage.py sqlmigrate application migration_number```
It is possible to override it.

3. Apply the migration on the database
```
  python manage.py migrate
```

### Manipulate data

**Create an instance of a model: **
```
instance=Model(field1=..., field2=...)
instance.save()
```
**Create a model2's instance with a ForeignKey link to a specific model1's instance:** ```instance1.model2_set.create(...)```

**Modify a field of an instance:** ```instance.field=new_value```

**List the instances of a model:** ```Model.objects.all()```
**Get a specific instance:** ```instance=Model.objects.get(id=index)```

  **Filter the instances returned:** ```Model.objects.filter(field[__option]=value)```
  - Options:
    - __startswith
    - __year

  **Order the instances returned:** ```Model.objects.order_by("-field")```
  
  **Delete an instance:** ```instance.delete()```

  **Documentation**: https://docs.djangoproject.com/en/5.1/topics/db/queries/#field-lookups-intro


## Django shell
The Django shell allows to use the free API given by Django

```
  python manage.py shell
```


## Django Admin

**Django admin** is an administratif interface to edit models content. 
Admin Site URL: serverURL/admin/ (default: http://127.0.0.1:8000/admin/) 

### Admin user

An **admin user** is a user that have the permission to access the admin interface.

**Create an admin user**: ```python manage.py createsuperuser```

### Add custom model in Admin
1. Register the model in *application*/admin.py:
```
  from django.contrib import admin
  from .models import model

  admin.site.register(model)
```