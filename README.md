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

## Django Project
### Start a project
```
  django-admin startproject merchex
```

### Launch the development server
```
  python manage.py runserver
```
To consult the server go to : http://127.0.0.1:8000/

### Create a database 
```
  python manage.py migrate
```
The database is save in the db.sqlite3 file

### Create an application
In Django an application is a sub-section of the project
```
  python manage.py startapp application_name
```


