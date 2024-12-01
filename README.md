# proyecto_tareas_python
Educativo y de Aprendizaje Personal

---

## Tabla de Contenidos
- [Requisitos](#requisitos)
- [Configuración del Entorno](#configuración-del-entorno)
- [Activación del Entorno](#Activación-del-Entorno)
- [Configuración Inicial](#configuración-inicial)
- [Pasos del Proyecto](#pasos-del-proyecto)
  - [Configuración del Proyecto](#configuración-del-proyecto)
  - [Creación de Vistas y Modelos](#creación-de-vistas-y-modelos)

---
## Requisitos

- Python 3.9 o superior
- Django 4.0 o superior

---
## Configuración del Entorno

1. Crear el entorno virtual:
   ```bash
   python -m venv entorno_virtual 

## Activación del Entorno

2. Activar el entorno virtual:
    ### Windows
    ```bash
    entorno_virtual\Scripts\activate

## Configuración Inicial
## Instalar Django y Guardar dependencias

3. Intalación Django
    ```bash
    pip install django

4. Instalamos la actualizacion de pip
    ```bash
    python.exe -m pip install --upgrade pip

## Guardar las dependencias
5. Instalación dependencias
    ```bash
   pip freeze > requirements.txt

## Pasos del Proyecto
6. Crear el Proyecto
    ```bash
    django-admin startproject proyecto

7. Ingresar al directorio del Proyecto
    ```bash
    cd proyecto

8. Creamos la Aplicación baseapp
    ```bash
    python manage.py startapp baseapp

## Configuración del Proyecto

9. Hacemos Migraciones para que cree las tablas por defecto que tiene django y creando el db.sqlite3
   ```bash 
   python manage.py migrate


10. Conectar el proyecto con la aplicación: Agregar 'baseapp.apps.BaseappConfig', en la lista INSTALLED_APPS dentro del archivo proyecto/settings.py

    ```bash
    INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'baseapp.apps.BaseappConfig',
    ]
11. En baseapp creamos la urls.py = baseapp/urls.py 
12. Configuramos la views que esta en baseapp/views.py
    ```bash
    from django.shortcuts import render
    from django.http import HttpResponse

    # Create your views here.
    def lista_pendientes(pedido):
        return HttpResponse("listas pendientes")

13. Volvemos baseapp/urls.py llamamos a la función listas_pendientes views. 
    ```bash
    from django.urls import path 
    from . import views

    urlpatterns = [path('',views.lista_pendientes, name="pendientes")]

14. Agregamos la ruta de baseapp.urls al proyecto/urls.py
    ```bash
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('',include('baseapp.urls')),]

15. Hacemos correr el Servidor = y nos dara como resultado lo que contenga la función de listas pendientes
    ```bash
    python manage.py runserver 

16. Generamos el modelo en baseapp/models.py
    ```bash
    from django.db import models
    from django.contrib.auth.models import User

    # Create your models here.
     
    class Tarea(models.Model):
        usuario = models.ForeignKey(User, on_delete=models.CASCADE,
                                    null=True,
                                    blank=True)
        titulo = models.CharField(max_length=200)
        descripcion = models.TextField( null=True,
                                    blank=True)
        completo = models.BooleanField(default=False)
        creado = models.DateTimeField(auto_now_add=True)
        
        def __str__(self):
            return self.titulo
        
        class Meta:
            ordering = ['completo']

17. Creamos la migracion para que nuestra tabla aparezca en la base de datos
    ```bash
    python manage.py makemigrations

18. este comando crea python manage.py makemigrations crea un archivo 0001_initial.py baseapp/migrations/0001_initial.py
    ```bash
    # Generated by Django 5.1.3 on 2024-12-01 04:42
    import django.db.models.deletion
    from django.conf import settings
    from django.db import migrations, models


    class Migration(migrations.Migration):

        initial = True

        dependencies = [
            migrations.swappable_dependency(settings.AUTH_USER_MODEL),
        ]

        operations = [
            migrations.CreateModel(
                name='Tarea',
                fields=[
                    ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                    ('titulo', models.CharField(max_length=200)),
                    ('descripcion', models.TextField(blank=True, null=True)),
                    ('completo', models.BooleanField(default=False)),
                    ('creado', models.DateTimeField(auto_now_add=True)),
                    ('usuario', models.ForeignKey(blank=True, null=True, on_delete=django.db.models.deletion.CASCADE, to=settings.AUTH_USER_MODEL)),
                ],
                options={
                    'ordering': ['completo'],
                },
            ),
        ]

19. Procedemos hacer la migración donde se veran los datos reflejado en db.sqlite3 y creara baseapp_tarea en la base de datos
    ```bash
    python manage.py migrate 