# Django Girls Valencia

## Welcome

Bienvenida a un taller para dar tus primeros pasos en el mundo de la programación o, en caso de ser una veterana en el lenguaje, la posibilidad para ampliar tus conocimientos en el lenguaje.

Tal vez estés asustada. ¡No te preocupes si no tienes experiencia previa picando código!, este taller está diseñado para que puedas seguirlo sin problemas. Además, estarás siempre acompañada por una mentora simpatiquísima que te guiará en todo momento.

Solo te pedimos 3 requisitos para empezar:

- Un ordenador con conexión a internet.
- Seguir las instrucciones de tu mentor.
- Ganas de aprender.

No esperamos que lo termines todo, pero si lo haces, ¡enhorabuena! Habrás creado tu primer blog con Django. Te animamos a que mantengas una comunicación fluida con tu mentor y le hagas todas las preguntas que necesites.

¿Estas preparada? ¡Vamos a ello!

### ¿Qué es Python? ¿Por qué Python?

Python es un lenguaje de programación interpretado cuya filosofía hace hincapié en una sintaxis que favorezca un código legible. Se trata de un lenguaje de programación multiparadigma, ya que soporta orientación a objetos, programación imperativa y, en menor medida, programación funcional. Lo único importante para ti es que es perfecto para principiantes y expertos, y es uno de los lenguajes más populares en la actualidad.

Sería una buena oportunidad para aprender lo más elemental (lee la expansión que hay un poco más abajo).

- Variables.
- Tipos de datos.
- Estructuras de control.
- Loops.
- Funciones.

En caso contrario, puedes saltarte esta parte.

- Preguntas que puedes hacer al mentor: ¿Qué es un lenguaje de programación interpretado? ¿Qué es un lenguaje de programación multiparadigma? ¿Qué otros lengaujes de programación son populares?

Expansiones:

- [Tutorial de Python](https://developer.mozilla.org/es/docs/Glossary/Python)

### ¿Qué es Django? ¿Por qué Django? Alternativas y sus diferencias

Django es un framework web para construir apicaciones web en Python (un blog, un ecommerce, una red social, etc). Lo usaremos para construir fácilmente nuestro blog.

- Preguntas que puedes hacer al mentor: ¿Qué es un framework web? ¿Qué otros frameworks web existen? ¿Por qué Django es tan popular?

Expansiones:

- [Tutorial de Django](https://developer.mozilla.org/es/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)

## Preparando el entorno de desarrollo

### Editor de código o IDE

Para este tutorial, usaremos [VS Code](https://code.visualstudio.com/), un editor popular y gratuito con buenas extensiones para Python y Django. Puedes usar cualquier otro editor o IDE que prefieras.

### Docker

Docker es una plataforma de software que permite a los desarrolladores, administradores de sistemas y otros usuarios empaquetar, distribuir y ejecutar aplicaciones en contenedores. Los contenedores son procesos independientes entre sí y pueden tener sus propias versiones de software, librerías y configuraciones. Muy útil para tener un entorno de desarrollo que no entre en conflicto con otros proyectos.

- Preguntas que puedes hacer al mentor: ¿Qué es un contenedor? ¿Qué es un entorno de desarrollo? ¿Qué es un conflicto de versiones?

Crea un archivo `Dockerfile` en la raíz de tu proyecto con el siguiente contenido:

```dockerlife
FROM python:3.12-slim

# Prevents Python from writing pyc files to disc (equivalent to python -B option)
ENV PYTHONDONTWRITEBYTECODE=1
# Prevents Python from buffering stdout and stderr (equivalent to python -u option)
ENV PYTHONUNBUFFERED=1

# Sets python path to be able to import modules
ENV PYTHONPATH=":/usr/src/app"
# set work directory
WORKDIR /usr/src/app

# set time
RUN ln -fs /usr/share/zoneinfo/Europe/Madrid /etc/localtime
RUN dpkg-reconfigure -f noninteractive tzdata

# install software
RUN apt-get update && apt-get install -y \
    build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# install dependencies
COPY requirements.txt .
RUN pip3 install --no-cache-dir --upgrade pip
RUN pip3 install --no-cache-dir -r requirements.txt
```

Acabas de definir una imagen. Sirven para crear contenedores. Un contenedor es una instancia de una imagen. Para crear un contenedor, necesitas un archivo `docker-compose.yml`.

Crea un archivo `docker-compose.yml` en la raíz de tu proyecto con el siguiente contenido:

```compose
services:

  django:
    build: .
    volumes:
      - .:/usr/src/app/
    ports:
    - "8000:8000"
```

Ya esta tu entorno de desarrollo listo.

- Preguntas que puedes hacer al mentor: ¿Qué es un archivo Dockerfile? ¿Qué es un archivo docker-compose.yml?

Expansiones:

- [Tutorial de Docker](https://www.freecodecamp.org/espanol/news/guia-de-docker-para-principiantes-como-crear-tu-primera-aplicacion-docker/)

## Creando un proyecto Django

Ahora vamos a crear un proyecto Django usando Docker.

```bash
docker compose run django django-admin startproject my_app .
docker compose run django python manage.py startapp my_blog
cd my_blog
```

- Preguntas que puedes hacer al mentor: ¿Qué es un proyecto Django? ¿Qué es una aplicación Django?


### Configuraciones básicas

Edita el archivo `my_blog/settings.py` para asegurarte de que las configuraciones de bases de datos, aplicaciones instaladas, y middleware sean correctas. En principio no necesitas cambiar nada, pero es bueno saber dónde están estas configuraciones.


### Base de datos

Django por defecto usa SQLite, un motor de base de datos ligero y fácil de usar. Para este tutorial, SQLite es suficiente. Si necesitas usar otro motor de base de datos, puedes cambiar la configuración en `my_blog/settings.py`.

- Preguntas que puedes hacer al mentor: ¿Qué es una base de datos? ¿Qué es un motor de base de datos? ¿Qué es SQLite? ¿Qué otros motores de base de datos existen?

### Levantando el servidor

Es el  momento de levantar el servidor de desarrollo de Django. Ejecuta el siguiente comando:

```bash
docker compose django python manage.py runserver
```

Abre tu navegador y ve a `http://localhost:8000/`. Deberías ver una página de bienvenida de Django.

Para automatizar el comando, podemos modificar el archivo `docker-compose.yml`:

```compose
services:

  django:
    build: .
    volumes:
      - .:/usr/src/app/
    ports:
    - "8000:8000"
    command: python manage.py runserver
```

Ahora solo necesitarás ejecutar `docker compose up` para levantar el servidor.

## Creando una aplicación

### Hola mundo

Eidta el archivo `my_blog/views.py` y añade el siguiente código:

```python
from django.http import HttpResponse

def hello_world(request):
    return HttpResponse('Hola, mundo!')
```

Ahora crea un archivo `urls.py` en la carpeta `my_blog` con el siguiente contenido:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
]
```

Y añade la nueva aplicación a las URLs del proyecto en `my_app/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('my_blog.urls')),
]
```

Ahora ve a `http://localhost:8000/` y deberías ver el mensaje `Hola, mundo!`.

Te felicito, acabas de crear tu primera aplicación Django. Tal vez un poco minimalista, pero es un buen comienzo.

Vamos a hacer algo más interesante: añadir información a la base de datos y mostrarla en la página.

- Preguntas que puedes hacer al mentor: ¿Qué es una vista? ¿Qué es una URL?

## Listado de artículos

### Creando un modelo para guardar la información en la base de datos

Django usa un ORM (Object-Relational Mapping) para interactuar con la base de datos. Esto significa que puedes usar Python para definir tus modelos y Django se encargará de crear las tablas en la base de datos. ¡Es super fácil!

Primero crea un modelo en `my_blog/models.py`:

```python
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    photo = models.ImageField(upload_to='photos/')
    description = models.TextField()
    date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

Ahora crea la migración y aplica los cambios a la base de datos:

```bash
docker compose run django python manage.py makemigrations
docker compose run django python manage.py migrate
```

- Preguntas que puedes hacer al mentor: ¿Qué es un modelo? ¿Qué es un ORM? ¿Qué es una migración?

La base de datos ya esta lista. Ahora vamos a añadir algunos artículos.

### Configurando Admin de Django

Para hacerlo fácil, vamos a usar el admin de Django. Añade el siguiente código a `my_blog/admin.py`:

```python
from django.contrib import admin

from .models import Article

admin.site.register(Article)
```

Ya se ha creado un panel administrativo para que puedas añadir, editar y borrar artículos.

Entra en `http://localhost:8000/admin/`.

¡Ups! ¿Cual es la contraseña? No te preocupes, vamos a crear un superusuario.

```bash
docker compose run django python manage.py createsuperuser
```

Sigue las instrucciones y ya puedes entrar en el panel administrativo.

Dentro crea algunos artículos. Nosotros te recomendamos que añadas alguna de las mujeres más importantes en la historia de la informática:

- Margaret Hamilton: https://es.wikipedia.org/wiki/Margaret_Hamilton_(cient%C3%ADfica)
- Grace Hopper: https://es.wikipedia.org/wiki/Grace_Hopper
- Mujeres ENIAC: https://es.wikipedia.org/wiki/ENIAC#Mujeres_programadoras_del_EN
- Ángela Ruiz Robles: https://es.wikipedia.org/wiki/%C3%81ngela_Ruiz_Robles

- Preguntas que puedes hacer al mentor: ¿Qué es un superusuario? ¿Qué es un panel administrativo?

Ahora necesitaremos mostrar estos artículos en la página principal.

### Mostrando los artículos en la página principal

Edita el archivo `my_blog/views.py` y sustituye todo el contenido con el siguiente código:

```python
from django.shortcuts import render
from .models import Article

def article_list(request):
    articles = Article.objects.all()
    return render(request, 'my_blog/article_list.html', {'articles': articles})
```

Crea un archivo `article_list.html` en la carpeta `my_blog/templates/my_blog` con el siguiente contenido:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Lista de artículos</title>
</head>
<body>
<h1>Lista de grandes mujeres en la historia de la informática</h1>
{% for article in articles %}
    <article>
        <h2>{{ article.title }}</h2>
        <img src="{{ article.photo.url }}" alt="{{ article.title }}">
        <p>{{ article.description }}</p>
    </article>
{% endfor %}
</body>
</html>
```

Y añade la nueva vista a las URLs de la aplicación en `my_blog/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.article_list, name='article_list'),
]
```

Ves a `http://localhost:8000/` y deberías ver los artículos que has añadido en el panel administrativo.

- Preguntas que puedes hacer al mentor: ¿Qué es HTML? ¿Qué es `for`? ¿De donde sale `article`?

Expansiones:

- [Tutorial de HTML por Mozilla](https://developer.mozilla.org/es/docs/Learn/HTML/Introduction_to_HTML).
- [Tutorial de HTML por Andros Fenollosa](https://programadorwebvalencia.com/cursos/html/).

### Decorando un poco la página con CSS

Vamos a añadir un poco de estilo a la página. Crea un archivo `style.css` en la carpeta `my_blog/static/css` con el siguiente contenido:

```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f0f0f0;
    color: #333;
}

h2 {
    color: #333;
}

article {
    background-color: #fff;
    margin: 10px;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

img {
    max-width: 100%;
    height: auto;
}
```

Y añade el archivo CSS a la plantilla `article_list.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Lista de artículos</title>
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
<h1>Lista de grandes mujeres en la historia de la informática</h1>
{% for article in articles %}
    <article>
        <h2>{{ article.title }}</h2>
        <img src="{{ article.photo.url }}" alt="{{ article.title }}">
        <p>{{ article.description }}</p>
    </article>
{% endfor %}
</body>
</html>
```

Vuelve a cargar la página y deberías ver los cambios.

¡Bien hecho!

- Preguntas que puedes hacer al mentor: ¿Qué es CSS? ¿Qué es `link`?

Expansiones:

- [Tutorial de CSS por Mozilla](https://developer.mozilla.org/es/docs/Learn/CSS/First_steps).
- [Tutorial de CSS por Andros Fenollosa](https://programadorwebvalencia.com/cursos/css/).

### Creando una página individual para cada artículo

Vamos a añadir una página individual para cada artículo. Edita el archivo `my_blog/views.py` y añade el siguiente código:

```python
def article_detail(request, pk):
    article = Article.objects.get(pk=pk)
    return render(request, 'my_blog/article_detail.html', {'article': article})
```

Crea un archivo `article_detail.html` en la carpeta `my_blog/templates/my_blog` con el siguiente contenido:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ article.title }}</title>
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
<h1>{{ article.title }}</h1>
<img src="{{ article.photo.url }}" alt="{{ article.title }}">
<p>{{ article.description }}</p>
</body>
</html>
```

Y añade la nueva vista a las URLs de la aplicación en `my_blog/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.article_list, name='article_list'),
    path('<int:pk>/', views.article_detail, name='article_detail'),
]
```

Para acceder a cada uno de los artículos, necesitas añadir un enlace en la plantilla `article_list.html`. Sustituiremos el contenido del artículo con el enlace para acceder a la página individual:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Lista de artículos</title>
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
<h1>Lista de grandes mujeres en la historia de la informática</h1>
{% for article in articles %}
    <article>
        <h2><a href="{% url 'article_detail' article.pk %}">{{ article.title }}</a></h2>
        <img src="{{ article.photo.url }}" alt="{{ article.title }}">
        <p><a href="{% url 'article_detail' article.pk %}">Leer más</a></p>
    </article>
{% endfor %}
</body>
</html>
```

Vuelve a cargar la página y deberías ver los enlaces a las páginas individuales de cada artículo. Al pulsar sobre ellos, deberías ver la página individual.

¡Enhorabuena! Has creado un blog con Django.

- Preguntas que puedes hacer al mentor: ¿Qué CSS deberíamos añadir para que los enlaces se vean como botones?

## Expandindo tu blog

Habla con tu mentor sobre cómo puedes expandir tu blog. Aquí tienes algunas ideas:

- Usar un sistema de plantillas para no repetir código HTML.
- Añadir un sistema de likes.
- Añadir un sistema de comentarios.
- Incorporar un buscador.
- [Crea un chat en tiempo real](https://programadorwebvalencia.com/django-chat-usando-websockets-con-salas-y-async/).
