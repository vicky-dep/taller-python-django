
# Taller de Python y Django

Repositorio de prácticas del taller de Python y Django.

Este documento recoge los comandos y procedimientos básicos para configurar el entorno de desarrollo en Linux, trabajar con Django y mantener el proyecto sincronizado entre distintos ordenadores mediante Git y GitHub.

---

## 1. Requisitos previos

Comprobar que Python está instalado:

```bash
python3 --version
```

Comprobar pip:

```bash
python3 -m pip --version
```

Comprobar si Django está instalado:

```bash
python3 -m django --version
```

> Django se instalará dentro de un entorno virtual para mantener las dependencias aisladas del sistema.

En Ubuntu/Debian, si faltan pip o venv:

```bash
sudo apt update
sudo apt install python3-pip python3-venv
```

---

## 2. Clonar el repositorio

Situarse en el directorio donde se quieran guardar los proyectos:

```bash
git clone URL_SSH_DEL_REPOSITORIO
cd taller-python-django
```

La URL SSH se obtiene desde GitHub, en **Code → SSH**.

> No es necesario crear previamente la carpeta del proyecto. Git la crea automáticamente.

---

## 3. Crear y activar el entorno virtual

El entorno virtual permite instalar paquetes de Python sin afectar al sistema operativo ni a otros proyectos.

### Crear el entorno

Desde la raíz del proyecto:

```bash
python3 -m venv venv
```

Esto genera una carpeta llamada `venv/`.

### Activar el entorno en Linux

```bash
source venv/bin/activate
```

Cuando esté activado, aparecerá `(venv)` en la terminal.

### Desactivar el entorno

```bash
deactivate
```

> El entorno virtual se crea una sola vez por ordenador, pero debe activarse cada vez que se abra una nueva terminal para trabajar en el proyecto.

---

## 4. Instalar paquetes con pip

Con el entorno virtual activado:

### Instalar Django

```bash
python -m pip install django
```

### Instalar otros paquetes

```bash
python -m pip install nombre_paquete
```

### Instalar una versión específica

```bash
python -m pip install django==5.2
```

### Consultar paquetes instalados

```bash
python -m pip list
```

### Consultar la versión de Django

```bash
python -m django --version
```

### Desinstalar un paquete

```bash
python -m pip uninstall nombre_paquete
```

---

## 5. Gestionar requirements.txt

El archivo `requirements.txt` contiene las dependencias necesarias para reproducir el entorno del proyecto.

### Generar o actualizar requirements.txt

Con el entorno virtual activado:

```bash
python -m pip freeze > requirements.txt
```

Este comando registra los paquetes instalados y sus versiones.

Ejemplo de contenido:

```text
asgiref==3.x.x
Django==5.x.x
sqlparse==0.x.x
```

Las versiones anteriores son ilustrativas.

### Instalar dependencias desde requirements.txt

Al clonar el proyecto en otro ordenador:

```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt
```

### Cuándo actualizar requirements.txt

Después de instalar, actualizar o eliminar paquetes:

```bash
python -m pip freeze > requirements.txt
```

> `requirements.txt` se sube a GitHub, pero la carpeta `venv/` no.

---

## 6. Crear un proyecto Django

Con Django instalado y el entorno virtual activado:

```bash
django-admin startproject config .
```

El punto final indica que el proyecto se crea en el directorio actual.

Estructura básica:

```text
taller-python-django/
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── manage.py
├── requirements.txt
└── venv/
```

### Archivos importantes

- `manage.py`: herramienta para administrar el proyecto.
- `settings.py`: configuración general.
- `urls.py`: definición de rutas.
- `asgi.py` y `wsgi.py`: puntos de entrada para desplegar la aplicación.

---

## 7. Crear una aplicación Django

Un proyecto Django puede contener varias aplicaciones.

Para crear una aplicación:

```bash
python manage.py startapp mi_app
```

Se genera una carpeta similar a:

```text
mi_app/
├── migrations/
│   └── __init__.py
├── __init__.py
├── admin.py
├── apps.py
├── models.py
├── tests.py
└── views.py
```

### Registrar la aplicación

Abrir `config/settings.py` y añadir la aplicación en `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'mi_app',
]
```

---

## 8. Crear modelos en Django

Los modelos representan los datos que se almacenarán en la base de datos.

Se definen en el archivo `models.py` de cada aplicación.

Ejemplo:

```python
from django.db import models

class Ejemplo(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True)
    fecha_creacion = models.DateTimeField(auto_now_add=True)
    activo = models.BooleanField(default=True)

    def __str__(self):
        return self.nombre
```

### Tipos de campos habituales

| Campo | Descripción |
|---|---|
| CharField | Texto corto |
| TextField | Texto largo |
| IntegerField | Número entero |
| FloatField | Número decimal aproximado |
| DecimalField | Número decimal de precisión fija |
| BooleanField | Verdadero o falso |
| DateField | Fecha |
| DateTimeField | Fecha y hora |
| EmailField | Correo electrónico |
| ForeignKey | Relación muchos a uno |
| ManyToManyField | Relación muchos a muchos |

Django crea automáticamente una clave primaria `id` si no se especifica otra.

---

## 9. Migraciones y base de datos

Django utiliza un ORM para trabajar con bases de datos mediante clases Python.

Por defecto, utiliza SQLite.

### Generar migraciones

```bash
python manage.py makemigrations
```

Detecta cambios en los modelos y genera los archivos de migración.

### Aplicar migraciones

```bash
python manage.py migrate
```

Aplica las migraciones a la base de datos.

### Consultar el estado de las migraciones

```bash
python manage.py showmigrations
```

### Flujo habitual

Después de modificar un modelo:

```bash
python manage.py makemigrations
python manage.py migrate
```

> Los archivos de migración generados dentro de `migrations/` deben incluirse en Git.

---

## 10. Ejecutar el servidor de Django

Desde la raíz del proyecto, con el entorno virtual activado:

```bash
python manage.py runserver
```

Abrir en el navegador:

http://127.0.0.1:8000/

Para detener el servidor:

```text
Ctrl + C
```

El servidor integrado de Django está pensado para desarrollo, no para producción.

---

## 11. Panel de administración de Django

Django incluye un panel de administración.

### Crear un superusuario

```bash
python manage.py createsuperuser
```

Introducir los datos solicitados.

### Acceder al panel

Arrancar el servidor:

```bash
python manage.py runserver
```

Abrir:

http://127.0.0.1:8000/admin/

### Registrar un modelo en el administrador

En `mi_app/admin.py`:

```python
from django.contrib import admin
from .models import Ejemplo

admin.site.register(Ejemplo)
```

El modelo aparecerá en el panel de administración.

---

## 12. Archivo .gitignore

El archivo `.gitignore` indica qué archivos y carpetas no deben incluirse en el repositorio.

Contenido recomendado:

```gitignore
# Entorno virtual
venv/

# Python
__pycache__/
*.py[cod]
.pytest_cache/

# Variables de entorno
.env
.env.*

# Base de datos local Django
db.sqlite3

# Archivos del sistema
.DS_Store
```

Comprobar qué archivos detecta Git:

```bash
git status
```

Comprobar si Git ignora el entorno virtual:

```bash
git check-ignore venv/
```

> Nunca subir contraseñas, claves privadas ni archivos con secretos.

---

## 13. Trabajar con Git y GitHub

### Consultar cambios

```bash
git status
```

### Añadir archivos

```bash
git add .
```

### Crear un commit

```bash
git commit -m "Descripción de los cambios"
```

### Subir cambios a GitHub

```bash
git push
```

### Descargar cambios

```bash
git pull
```

### Comprobar el repositorio remoto

```bash
git remote -v
```

### Consultar historial

```bash
git log --oneline
```

---

## 14. Trabajar desde otro ordenador

### Primera vez en la escuela

Clonar el repositorio:

```bash
git clone URL_SSH_DEL_REPOSITORIO
cd taller-python-django
```

Crear y activar el entorno virtual:

```bash
python3 -m venv venv
source venv/bin/activate
```

Instalar las dependencias existentes:

```bash
python -m pip install -r requirements.txt
```

Si todavía no hay dependencias, instalar las que indique el profesor.

### Al terminar la clase

Actualizar las dependencias:

```bash
python -m pip freeze > requirements.txt
```

Comprobar los cambios:

```bash
git status
```

Guardar y subir:

```bash
git add .
git commit -m "Update Django workshop"
git push
```

### Al volver a casa

Actualizar el repositorio:

```bash
git pull
```

Activar el entorno virtual:

```bash
source venv/bin/activate
```

Sincronizar las dependencias:

```bash
python -m pip install -r requirements.txt
```

> Git sincroniza el código y los archivos de configuración, pero no sincroniza automáticamente los paquetes instalados dentro de `venv/`.

---

## 15. Comandos útiles de Django

| Comando | Función |
|---|---|
| `django-admin startproject config .` | Crear proyecto |
| `python manage.py startapp mi_app` | Crear aplicación |
| `python manage.py runserver` | Iniciar servidor |
| `python manage.py makemigrations` | Generar migraciones |
| `python manage.py migrate` | Aplicar migraciones |
| `python manage.py showmigrations` | Ver migraciones |
| `python manage.py createsuperuser` | Crear administrador |
| `python manage.py shell` | Abrir consola interactiva |
| `python manage.py check` | Comprobar configuración |

---

## 16. Errores frecuentes

### django: command not found

Comprobar que el entorno virtual está activado:

```bash
source venv/bin/activate
```

Instalar Django si es necesario:

```bash
python -m pip install django
```

### No module named django

Django no está instalado en el intérprete Python que se está utilizando.

Comprobar:

```bash
which python
python -m pip list
```

### No module named venv

En Ubuntu/Debian puede faltar el paquete correspondiente:

```bash
sudo apt install python3-venv
```

### No changes detected

El comando `makemigrations` no detecta cambios en los modelos.

Comprobar que:

- La aplicación está registrada en `INSTALLED_APPS`.
- El modelo está definido correctamente.
- Se han guardado los cambios.

### Port is already in use

El puerto 8000 está ocupado.

Se puede utilizar otro:

```bash
python manage.py runserver 8001
```

### VS Code no reconoce los paquetes instalados

Seleccionar el intérprete del entorno virtual:

1. Abrir la paleta de comandos.
2. Buscar `Python: Select Interpreter`.
3. Seleccionar el intérprete ubicado en `venv/bin/python`.

---

## 17. Resumen rápido

### Crear entorno virtual

```bash
python3 -m venv venv
source venv/bin/activate
```

### Instalar Django

```bash
python -m pip install django
```

### Crear proyecto y aplicación

```bash
django-admin startproject config .
python manage.py startapp mi_app
```

### Migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

### Ejecutar servidor

```bash
python manage.py runserver
```

### Actualizar dependencias

```bash
python -m pip freeze > requirements.txt
```

### Guardar cambios en GitHub

```bash
git add .
git commit -m "Update project"
git push
```

---

## 18. Documentación oficial

- Python: https://docs.python.org/3/
- Django: https://docs.djangoproject.com/
- pip: https://pip.pypa.io/
- Git: https://git-scm.com/doc

---

**Taller de Python y Django — DAW**