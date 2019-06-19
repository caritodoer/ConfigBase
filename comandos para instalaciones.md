# Instalacion de base
Una vez instalado Ubuntu, en recomendable update/upgrade

## Update/Upgrade
```
$ sudo apt-get update
$ sudo apt-get upgrade
```
## Instalar Python 3 
Viene instalado por defecto. Para corroborar la version, en la terminal escribir:
```
	python3
	exit() --> para salir
```

## Para instalar pip3
PIP 3 nos sirve para descargar programas relacionados con Python3, para Python2 se necesita 'pip'
EN TERMINAL:
```
	sudo apt install python3-pip
```
para ver si se instalo correctamente, y para ver la version:
```
	pip3 -V
```
Para que descargue correctamente otros paquetes, es bueno tenerlo actualizado:
```
	pip3 install --upgrade pip    
```
**--->>> ESTO EN KUBUNTU Y OTRAS DISTRIBUCIONES DA ERRORES! NO ACTUALIZAR!**

## Para instalar Virtual Environement
```
$ sudo apt-get install python-pip python-virtualenv python-setuptools python-dev build-essential  python3
$ sudo pip install virtualenv 
$ mkdir Dev
$ cd Dev
$ mkdir venv && cd venv
$ virtualenv -p python3 .
$ cd Dev/venv/ #created above
$ source bin/activate
```
If activated correctly, you should see:
```
(venv) $
```
Check python version:
```
(venv) $ python --version #should return Python 3.6
```
To deactivate:
```
(venv) $ deactivate
```
To reactivate:
```
$ /path/to/your/virtual/env/
$ source bin/activate
```

## Para Instalar GIT
```
sudo apt install git
```

### Si ya existe el repositorio:
```
git clone https://github.com/caritodoer/nombreRepositorio.git
```
Esto crea una carpeta con el nombre del repositorio donde se descarga el mismo.
Despues de modificar archivos, pueden subirse de esta manera:
```
git add .
git commit -m "Algun comentario sobre las modificaciones"
git push
```

## Instalar Django
### Para Instalar **Django 2.0.7** & Start New project
```
$ /path/to/your/virtual/env/here/
$ source bin/activate
(here) $ pip install django==2.0.7
(here) $  mkdir src && cd src
(here) $ django-admin.py startproject cfehome .
(here) $ python manage.py migrate
(here) $ python manage.py createsuperuser
```

### Para instalar **django 1.10** 
Una vez que tenemos pip3, podemos instalar Django:
```
	pip3 install django==1.10
```
Lo ponemos con == porque queremos descargar ESA version de Django en especifico.

### Para instalar la **ultima version** de django:
```
	pip3 install Django
```
### Para corroborar qué versión de Django se instaló:
```
	python3 -m django --version
```

## Para Instalar PostgreSQL

EN TERMINAL	
```
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```
Esto instala postgresql y crea un usuario llamado postgres por defecto. Para entrar a esta cuenta escribir:
```
sudo -u postgres psql
```

### para setear la contraseña (o cambiarla despues en caso de olvido):
```
alter user postgres with password 'NuevaClave';
```
para grabar y salir:
```
\q 
```
para salir a la consola:
```
exit
``` 

### Instalar biblioteca de python para conectarse a postgres
```
sudo pip3 install psycopg2
```
Despues de esto hay que instalar el FrontEnd pgAdmin III, para trabajar ahi sin tener que hacerlo desde la consola.
Al abrir pgAdmin, hacer clic en el dibujo del enchufe (Nueva Conexion).
	* Name: MiConexion
	* HOST: 127.0.0.1 (Si es BDD local. Si es BDD remota, pongo la IP de esa maquina remota)
	* PORT: 5432 (ya esta determinado)
	* Servicio: (Queda vacio.)
	* Username: postgres
	* Password: postgres (lo que estabeci cuando lo instale)

### Otra forma de entrar a postgres es:
```
	sudo -i -u postgres
```
y dentro de postres escribir solo
```
	psql
```
para entrar en el shell de postgres.

### Para crear otro usuario puede ser directo:
```
	sudo -u pastgres createuser --interactive
```
o, una vez dentro de postgres:
```
	createsuperuser --interactive
```
Luego introducir el nombre, y 'y' para darle rol de superusuario

### Para crear una nueva Base de Datos
Al crear un nuevo usuario, Postgres crea por defecto una BDD con el mismo nombre que el usuario. Para crear una BDD apropiada, si estas logueado en postgres:
```
	createdb NombreBDD
```
Si no estas logueado, se puede hacer con sudo:
```
	sudo -u postgres createdb NombreBDD
```

## SASS: Preprocesador de CSS3
Instalacion usando Ruby
Luego, en consola:
```
sass --watch carpetaDeOrigen : carpetaDeDestino
```

# Pasos para iniciar un proyecto en django

0. TERMINAL y carpetas : Nos posicionamos donde quiero crear el proyecto
1. TERMINAL> django-admin startproject nombreProyecto
2. POSTGRESQL> Crear Base de Datos >> ej> Proyecto_db
3. SETTINGS.PY>
	DATABASES = {
		'default': {
			'ENGINE': 'django.db.backends.postgresql_psycopg2',
			'NAME': 'Proyecto_db', >> nombre de la base de datos
			'USER': 'postgres',
			'PASSWORD': 'miClaveDePostgres', >> postgres o caro1234 ?
			'HOST': 'localhost',
			'PORT': '5432',
 		}
	}

4. TERMINAL> python3 manage.py startapp NombreApp
5. SETTINGS.PY>
	INSTALLED_APPS = {
		...
		...
		'NombreApp',
	} 
6. MODELS.PY> Crear modelos. Por ej.
	class NombreModelo(models.Modelo):
		nombre = models.CharField(max_length=30)
		...
		...
		def __str__(self):
			return self.nombre
7. TERMINAL> python3 manage.py makemigrations
8. TERMINAL> python3 manage.py migrate
9. TERMINAL> python3 manage.py createsuperuser
	ingresar nombre de usuario (ej. admin)
	ingresar password (ej. admin1234)
10. ADMIN.PY> agregar:
	from NombreApp.models import NombreModelo
	admin.site.register(NombreModelo)
11. TERMINAL> python3 manage.py runserver (se sale con Ctrl+C)
12. NAVEGADOR> localhost:8000/admin

#### VISTAS
13. dentro de la carpeta de la app, abrir views.py
ejemplo de views simple:

from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world.")

#### URLS
14. dentro de la carpeta de la app, crear un archivo urls.py y añadir el siguiente codigo para vincular views con urls
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

15. en el arhivo urls.py del proyecto, vincular URLs de aplicaciones con URL general del proyecto
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('nombreApp/', include('nombreApp.urls')),
    path('admin/', admin.site.urls),
]

para corroborar que funciona ir al navegador y colocar http://localhost:8000/nombreApp/ y se debería ver el texto «Hello, world.» que escribimos en views.py, en el views "index"

***

# Otras librerias
## Para importaciones y exportaciones a excel:
	libreria django-import-export
```
pip3 install django-import-export
```
http://django-import-export.readthedocs.io/en/latest/getting_started.html

## Para formularios
	libreria crispy-forms
```
pip3 install --upgrade django-crispy-forms
```
## Para crear PDF:
	libreria Reportlab
```
pip3 install reportlab
```
http://django-import-export.readthedocs.io/en/latest/getting_started.html
