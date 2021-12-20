# Ejemplo 01

En este ejercicio vamos a crear nuestro primer projecto Django.


## Pasos 1. crear el proyecto

Crear un Proyecto llamado `Alumnos` usando el comando siguiente

```sh
# django-admin startproject Alumnos
```

NOTA: Puedes usar el temaplate de django para VS [Link](https://github.com/ElDwarf/Django-project-vs-template)


## Paso 2. Migrar la base de datos


```sh
# python manage.py migrate
```

## Paso 3. Crear un Super Usuario y verificar en el 

1. Crear super-usuario
```sh
# python manage.py createsuperuser
```

2. Ejecutar servidor y acceder a admin
```sh
python manage.py runserver
```

3. Ingrese al servidor `http://127.0.0.1:8000/admin`

## Paso 3. Crear una aplicacion con un modelo 

```sh
django-admin startapp alumnos
```

NOTA: No te olvides de agregar la app en el setting del proyecto

## Paso 4. Agregar Modelo

Codigo ejemplo del modelo

```Python
from django.db import models


class Alumno(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    birthday = models.DateTimeField(null=True, blank=True)

    def __str__(self):
        return f'{self.last_name.title()}, {self.first_name.title()}'
```

## Paso 5. Crear las Vista 

Codigo de ejemplo la vista

```python
from django.http import HttpResponse, JsonResponse
import datetime

from .models import Alumno


def add_alumno(request):
    Alumno.objects.create(first_name=request.GET.get('name', '-'),
                          last_name=request.GET.get('lastname', '-'))
    response = {'status': 'OK'}
    return JsonResponse(response)


def get_alumnos(request):
    alumnos = Alumno.objects.all()
    html = '<html><body><h1>Alumnos<H1>'
    for item in alumnos:
        html += f'<p><a href="/alumnos/{item.id}">{item.last_name}, {item.first_name}</a></p>'
    html += '</body></html>'
    return HttpResponse(html)


def alumno(request, id_alumno):
    alumno = Alumno.objects.get(pk=id_alumno)
    html = '<html><body><h1>Alumnos<H1>'
    html += f'<p>Apellido: {alumno.last_name}</p>'
    html += f'<p>Nombre: {alumno.first_name}</p>' 
    html += '</body></html>'
    return HttpResponse(html)
```

NOTA: No te olvides de agregar las URL.

## Paseo 6. Subir el proyecto.
