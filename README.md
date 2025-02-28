# Django Girls Valencia

# Tabla de Contenidos

1. [Bienvenida](#welcome)
2. [¿Qué es Python? ¿Por qué Python?](#que-es-python)
3. [¿Qué es Django? ¿Por qué Django? Alternativas y sus diferencias](#que-es-django)
4. [Preparando el entorno de desarrollo](#preparando-el-entorno-de-desarrollo)  
   4.1 [Editor de código o IDE](#editor-de-código-o-ide)  
   4.2 [Docker](#docker)  
5. [Creando un proyecto Django](#creando-un-proyecto-django)  
   5.1 [Configuraciones básicas](#configuraciones-básicas)  
   5.2 [Base de datos](#base-de-datos)  
   5.3 [Levantando el servidor](#levantando-el-servidor)  
6. [Creando una aplicación](#creando-una-aplicación)  
   6.1 [Hola Mundo](#hola-mundo)  
7. [Listado de artículos](#listado-de-artículos)  
   7.1 [Creando un modelo para guardar la información en la base de datos](#creando-un-modelo-para-guardar-la-información-en-la-base-de-datos)  
   7.2 [Configurando Admin de Django](#configurando-admin-de-django)  
   7.3 [Mostrando los artículos en la página principal](#mostrando-los-artículos-en-la-página-principal)  
   7.4 [Mostrando imágenes](#mostrando-imágenes)  
   7.5 [Decorando un poco la página con CSS](#decorando-un-poco-la-página-con-css)  
   7.6 [Creando una página individual para cada artículo](#creando-una-página-individual-para-cada-artículo)
8. [Publica tu web en internet](#publica-tu-web-en-internet)
9. [Expandiendo tu blog](#expandiendo-tu-blog)

## Welcome

Bienvenida a un taller para dar tus primeros pasos en el mundo de la programación o, en caso de ser una veterana en el lenguaje, la posibilidad para ampliar tus conocimientos en el lenguaje.

Tal vez estés asustada. ¡No te preocupes si no tienes experiencia previa picando código!, este taller está diseñado para que puedas seguirlo sin problemas. Además, estarás siempre acompañada por una mentora simpatiquísima que te guiará en todo momento.

Solo te pedimos 3 requisitos para empezar:

- Un ordenador con conexión a internet.
- Seguir las instrucciones de tu mentora.
- Ganas de aprender.

No esperamos que lo termines todo, pero si lo haces, ¡enhorabuena! Habrás creado tu primer blog con Django. Te animamos a que mantengas una comunicación fluida con tu mentora y le hagas todas las preguntas que necesites.

¿Estás preparada? ¡Vamos a ello!

<div id='que-es-python'>

## ¿Qué es Python? ¿Por qué Python?

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

<div id='que-es-django' />

### ¿Qué es Django? ¿Por qué Django? Alternativas y sus diferencias

Django es un framework web para construir apicaciones web en Python (un blog, un ecommerce, una red social, etc). Lo usaremos para construir fácilmente nuestro blog.

- Preguntas que puedes hacer a tu mentora: ¿Qué es un framework web? ¿Qué otros frameworks web existen? ¿Por qué Django es tan popular?

Expansiones:

- [Tutorial de Django](https://developer.mozilla.org/es/docs/Learn_web_development/Extensions/Server-side/Django/Introduction)

## Preparando el entorno de desarrollo

### Editor de código o IDE

Para este tutorial, usaremos [VS Code](https://code.visualstudio.com/), un editor popular y gratuito con buenas extensiones para Python y Django. Puedes usar cualquier otro editor o IDE que prefieras.

### Docker

Docker es una plataforma de software que permite a los desarrolladores, administradores de sistemas y otros usuarios empaquetar, distribuir y ejecutar aplicaciones en contenedores. Los contenedores son procesos independientes entre sí y pueden tener sus propias versiones de software, librerías y configuraciones. Muy útil para tener un entorno de desarrollo que no entre en conflicto con otros proyectos.

- Preguntas que puedes hacer a tu mentora: ¿Qué es un contenedor? ¿Qué es un entorno de desarrollo? ¿Qué es un conflicto de versiones?

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
COPY . .
RUN pip3 install --no-cache-dir --upgrade pip
RUN pip3 install --no-cache-dir -r requirements.txt

CMD ["python", "manage.py", "runserver", "0.0.0.0:10000"]
```

Acabas de definir una imagen. Sirven para crear contenedores. Un contenedor es una instancia de una imagen. Para crear un contenedor, necesitas un archivo `compose.yml`.

Crea un archivo `compose.yml` en la raíz de tu proyecto con el siguiente contenido:

```compose
services:

  django:
    build: .
    volumes:
      - .:/usr/src/app/
    ports:
    - "10000:10000"
```

Ya está tu entorno de desarrollo listo.

- Preguntas que puedes hacer a tu mentora: ¿Qué es un archivo `Dockerfile`? ¿Qué es un archivo `compose.yml`?

Expansiones:

- [Tutorial de Docker](https://www.freecodecamp.org/espanol/news/guia-de-docker-para-principiantes-como-crear-tu-primera-aplicacion-docker/)

## Creando un proyecto Django

Ahora vamos a crear un proyecto Django usando Docker.

Primero crea un archivo `requirements.txt` en la raíz de tu proyecto con el siguiente contenido:

```txt
Django
pillow
```

Estamos definiendo las dependencias de nuestro proyecto. En este caso, Django (el framework web) y Pillow (una librería para trabajar con imágenes).

Ahora crea un proyecto Django:

```bash
docker compose run django django-admin startproject my_app .
docker compose run django python manage.py startapp my_blog
cd my_blog
```

Si tienes problemas con docker, podrás hacerlo [instalando python](https://www.python.org/downloads/)
```bash
pip install -r requirements.txt
django-admin startproject my_app . # Linux o Mac
django-admin.exe startproject my_app . # Windows
python manage.py startapp my_blog
```


- Preguntas que puedes hacer a tu mentora: ¿Qué es un proyecto Django? ¿Qué es una aplicación Django? ¿Por qué necesitamos un archivo `requirements.txt`?


### Configuraciones básicas

Edita el archivo `my_app/settings.py` para asegurarte de que las configuraciones de bases de datos, aplicaciones instaladas, y middleware sean correctas. En principio no necesitas cambiar nada, pero es bueno saber dónde están estas configuraciones.


### Base de datos

Una base de datos es un sistema de almacenamiento de información. Piensa en ella como una hoja de cálculo, o tabla gigante, donde puedes guardar y recuperar información de forma ordenada. Por ejemplo, puedes guardar cada artículo de tu blog en una fila. No solo tendrá contenido, sino también otros datos importantes como el título, la fecha de creación, las etiquetas, etc. Cada fila es un registro en la base de datos con columnas. Para recoperar último artículo que has añadido, puedes hacer una consulta a la base de datos pidiendo que te dé el último registro, o con la fecha más reciente, o con el título más largo, etc.

Django por defecto usa SQLite, un motor de base de datos ligero y fácil de usar. Para este tutorial, SQLite es suficiente. Si necesitas usar otro motor de base de datos, puedes cambiar la configuración en `my_app/settings.py`. Django soporta muchos motores de base de datos, como PostgreSQL, MySQL, Oracle, etc. No es necesario modificar nada.

- Preguntas que puedes hacer a tu mentora: ¿Qué es una base de datos? ¿Qué es un motor de base de datos? ¿Qué es SQLite? ¿Qué otros motores de base de datos existen?

Expansiones:

- [Tutorial de SQL resolviendo crímenes](https://www.sqlnoir.com/)
- [Tutorial de SQL y SQLite](https://programadorwebvalencia.com/cursos/sql/)

### Levantando el servidor

Es el  momento de levantar el servidor de desarrollo de Django.

Primero debemos editar el archivo `compose.yml` para que Django se quede funcionando:

```compose
services:

  django:
    build: .
    volumes:
      - .:/usr/src/app/
    ports:
    - "10000:10000"
    command: python manage.py runserver 0.0.0.0:10000
```

Ahora levanta el servidor:

```bash
docker compose up
```

Si no tienes docker:

```bash
python manage.py runserver 
```


Abre tu navegador y ve a `http://localhost:10000/`. Deberías ver una página de bienvenida de Django.


¡Enhorabuena! Django ya está funcionando en tu ordenador.

## Creando una aplicación

Antes de continuar necesito que ejecutes el siguiente comando en la carpeta de tu proyecto:

```shell
sudo chown $USER:$USER -R .
```

De este modo podrás editar los archivos sin problemas.

¡Ya no te distraigo más! Continuemos.

### Hola mundo

Edita el archivo `my_blog/views.py` y añade el siguiente código:

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
    path('', include('my_blog.urls')),
    path('admin/', admin.site.urls),
]
```

Ahora ve a `http://localhost:10000/` y deberías ver el mensaje `Hola, mundo!`.

Te felicito, acabas de crear tu primera aplicación Django. Tal vez un poco minimalista, pero es un buen comienzo.

Vamos a hacer algo más interesante: añadir información a la base de datos y mostrarla en la página.

- Preguntas que puedes hacer a tu mentora: ¿Qué es una vista? ¿Qué es una URL?

## Listado de artículos

### Creando un modelo para guardar la información en la base de datos

Django usa un ORM (Object-Relational Mapping) para interactuar con la base de datos. Esto significa que puedes usar Python para definir tus modelos y Django se encargará de crear las tablas en la base de datos. ¡Es super fácil!

Primero debemos informar a Django de nuestra nueva aplicación. Añade `my_blog` a la lista de aplicaciones instaladas en `my_app/settings.py`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'my_blog', # Nuevo
]
```

Es el momento de crea un modelo en `my_blog/models.py`:

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

Los modelos son clases que representan tablas en la base de datos. Cada atributo de la clase es un campo en la tabla. En este caso, hemos creado un modelo `Article` con los campos `title`, `photo`, `description` y `date`.

Ahora crea la migración y aplica los cambios a la base de datos. Abre otro terminal y ejecuta los siguientes comandos:

```bash
docker compose run django python manage.py makemigrations
docker compose run django python manage.py migrate
```

Si no tienes docker:
```bash
python manage.py makemigrations
python manage.py migrate
```


- Preguntas que puedes hacer a tu mentora: ¿Qué es un modelo? ¿Qué es un ORM? ¿Qué es una migración?



La base de datos ya está lista. Ahora vamos a añadir algunos artículos.

### Configurando Admin de Django

Para hacerlo fácil, vamos a usar el admin de Django. Añade el siguiente código a `my_blog/admin.py`:

```python
from django.contrib import admin

from .models import Article

admin.site.register(Article)
```

Ya se ha creado un panel administrativo para que puedas añadir, editar y borrar artículos.

Entra en `http://localhost:10000/admin/`.

¡Ups! ¿Cual es la contraseña? No te preocupes, vamos a crear un superusuario.

```bash
docker compose run django python manage.py createsuperuser
```

Si no tienes docker:

```bash
python manage.py createsuperuser
```


Sigue las instrucciones y ya puedes entrar en el panel administrativo. Puedes inventarte el email, lo importante es que recuerdes el usuario y la contraseña. Tranquila con tus datos, todo lo que pase entre tú y Django, se queda en tu ordenador. Estás guardando toda la información en tu base de datos local, en concreto en un archivo llamado `db.sqlite3` en la raíz de tu proyecto.


Ya puedes añadir algunos artículos. Vuelve a `http://localhost:10000/admin/`, usa el usuario y la contraseña que has creado y añade algunos artículos.

Nosotras te recomendamos que añadas alguna de las mujeres más importantes en la historia de la informática:

- Margaret Hamilton: https://es.wikipedia.org/wiki/Margaret_Hamilton_(cient%C3%ADfica)
- Grace Hopper: https://es.wikipedia.org/wiki/Grace_Hopper
- Mujeres ENIAC: https://es.wikipedia.org/wiki/ENIAC#Mujeres_programadoras_del_EN
- Ángela Ruiz Robles: https://es.wikipedia.org/wiki/%C3%81ngela_Ruiz_Robles

- Preguntas que puedes hacer a tu mentora: ¿Qué es un superusuario? ¿Qué es un panel administrativo?

Ahora necesitaremos mostrar estos artículos en la página principal.

### Mostrando los artículos en la página principal

Edita el archivo `my_blog/views.py` y sustituye todo el contenido con el siguiente código:

```python
from django.shortcuts import render
from django.http import HttpResponse
from .models import Article # Nuevo

def hello_world(request):
    return HttpResponse('Hola, mundo!')

def article_list(request): # Nuevo
    articles = Article.objects.all() # Nuevo
    return render(request, 'article_list.html', {'articles': articles}) # Nuevo
```

Crea un archivo `article_list.html` en la carpeta `my_blog/templates/` (deberás crearla) con el siguiente contenido:

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
    path('articles/', views.article_list, name='article_list'), # Nuevo
]
```

Ves a `http://localhost:10000/articles/` y deberías ver los artículos que has añadido en el panel administrativo.

Un momento... ¿por qué no se ven las imágenes? En el siguiente apartado lo solucionaremos.

- Preguntas que puedes hacer a tu mentora: ¿Qué es HTML? ¿Qué es `for`? ¿De donde sale `article`?

Expansiones:

- [Tutorial de HTML por Mozilla](https://developer.mozilla.org/es/docs/Learn/HTML/Introduction_to_HTML).
- [Tutorial de HTML por Andros Fenollosa](https://programadorwebvalencia.com/cursos/html/).

### Mostrando imágenes

Todo el contenido que subamos a la base de datos se guardará en la carpeta `media`. Para que Django pueda servir estos archivos, necesitamos añadir las rutas a `my_app/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings # Nuevo
from django.conf.urls.static import static # Nuevo

urlpatterns = [
    path('', include('my_blog.urls')),
    path('admin/', admin.site.urls),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Nuevo
```

Ahora ya deberías ver las imágenes en la página.


### Decorando un poco la página con CSS

Vamos a añadir un poco de estilo a la página. Crea un archivo `style.css` en la carpeta `static/css` (deberás crear ambas carpetas) con el siguiente contenido:

```css
:root {
    /* Paleta de colores vibrante y moderna */
    --primary-color: #FF7EB6;    /* Rosa moderno */
    --secondary-color: #7AF2FF;  /* Azul claro */
    --accent-color: #FF65A3;     /* Rosa más intenso */
    --background-gradient: linear-gradient(45deg, #fff5f5 0%, #f7f9ff 100%);
    --text-color: #2E2E2E;
    --soft-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
    --spacing-unit: 1.5rem;
    --border-radius: 20px;
    --transition-fast: 0.3s ease;
}

body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    margin: 0;
    padding: var(--spacing-unit) 0;
    background-image: var(--background-gradient);
    color: var(--text-color);
    line-height: 1.6;
}

h1 {
    text-align: center;
    font-size: 2.8rem;
    margin: 1.5em 0;
    color: var(--primary-color);
    text-shadow: 2px 2px 0 rgba(255, 126, 182, 0.1);
    position: relative;
}

h1::after {
    content: '';
    display: block;
    width: 80px;
    height: 4px;
    background: var(--accent-color);
    margin: 0.5em auto;
    border-radius: 2px;
}

h2 {
    color: var(--text-color);
    font-size: 1.8rem;
    margin-bottom: 1em;
    position: relative;
    padding-left: 1rem;
}

h2::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0.35em;
    height: 0.8em;
    width: 4px;
    background: var(--secondary-color);
    border-radius: 2px;
}

article {
    background: rgba(255, 255, 255, 0.9);
    backdrop-filter: blur(8px);
    margin: var(--spacing-unit) auto;
    padding: calc(var(--spacing-unit) * 1.5);
    border-radius: var(--border-radius);
    box-shadow: var(--soft-shadow);
    max-width: 800px;
    transition: transform var(--transition-fast), box-shadow var(--transition-fast);
}

article:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

img {
    max-width: 100%;
    height: auto;
    border-radius: 12px;
    margin: var(--spacing-unit) 0;
    transition: transform var(--transition-fast);
}

img:hover {
    transform: scale(1.02);
}

/* Efectos modernos */
@media (prefers-reduced-motion: no-preference) {
    article {
        animation: float 3s ease-in-out infinite;
    }

    @keyframes float {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-10px); }
    }
}

/* Mejoras de tipografía */
p {
    font-size: 1.1rem;
    letter-spacing: 0.02em;
}

/* Personalización de enlaces */
a {
    color: var(--accent-color);
    text-decoration: none;
    position: relative;
}

a::after {
    content: '';
    position: absolute;
    bottom: -2px;
    left: 0;
    width: 0;
    height: 2px;
    background: currentColor;
    transition: width var(--transition-fast);
}

a:hover::after {
    width: 100%;
}
```

Para que el archivo de estilos sea encontrado por el navegador, necesitaremos añadir a la plantilla HTML un par de lineas.

Al inicio del archivo `article_list.html`, añade `{% load static %` y  `<link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">` en la etiqueta `head`:

```html
{% load static %}
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

Por último, dile a Django que sirva los archivos estáticos. Añade la siguiente línea al final de `my_app/settings.py`:

```python
STATICFILES_DIRS = [BASE_DIR / 'static']
```

Vuelve a cargar la página y deberías ver los cambios.

¡Bien hecho!

- Preguntas que puedes hacer a tu mentora: ¿Qué es CSS? ¿Qué es `link`? ¿Qué es `static`? ¿Qué es `load`?

Expansiones:

- [Tutorial de CSS por Mozilla](https://developer.mozilla.org/es/docs/Learn/CSS/First_steps).
- [Tutorial de CSS por Andros Fenollosa](https://programadorwebvalencia.com/cursos/css/).

### Creando una página individual para cada artículo

Vamos a añadir una página individual para cada artículo. Edita el archivo `my_blog/views.py` y añade el siguiente código al final del mismo:

```python
def article_detail(request, pk):
    article = Article.objects.get(pk=pk)
    return render(request, 'article_detail.html', {'article': article})
```

Crea un archivo `article_detail.html` en la carpeta `my_blog/templates` con el siguiente contenido:

```html
{% load static %}
<!DOCTYPE html>
<html>
<head>
    <title>{{ article.title }}</title>
    <link rel="stylesheet" type="text/css" href="{% static 'css/style.css' %}">
</head>
<body>
    <article>
	<h1>{{ article.title }}</h1>
	<img src="{{ article.photo.url }}" alt="{{ article.title }}">
	<p>{{ article.description }}</p>
    </article>
</body>
</html>

```

Y añade la nueva vista a las URLs de la aplicación en `my_blog/urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.hello_world, name='hello_world'),
    path('articles/', views.article_list, name='article_list'),
    path('<int:pk>/', views.article_detail, name='article_detail'), # Nuevo
]

```

Para acceder a cada uno de los artículos, necesitas añadir un enlace en la plantilla `article_list.html`. Sustituiremos el contenido del artículo con el enlace para acceder a la página individual:

```html
{% load static %}
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

Fíjate en la URL. ¿Qué ves? ¿Qué crees que significa el número?

Te dejo un par de retos:

- ¿Cómo podrías cambiar la URL para que en lugar de un número, se vea el título del artículo?
- ¿Cómo podrías añadir un enlace para volver a la lista de artículos?

Te recomiendo hacer una busqueda en internet para encontrar la solución. Si no encuentras nada, pregunta a tu mentora.

¡Enhorabuena! Has creado un blog con Django.

- Preguntas que puedes hacer a tu mentora: ¿Qué CSS deberíamos añadir para que los enlaces se vean como botones?

## Publica tu web en internet

Necesitas una cuenta en [dockerhub](https://hub.docker.com/) y [render](https://render.com/)

Al crear tu cuenta en dockerhub crearás un nombre de usuario, usa ese nombre de usuario en los siguientes pasos.

Pon el nombre de usuario en el archivo settings.py en la variable DOCKER_USER y añade este código al final:

```python
DOCKER_USER = "adalovelace"

DOMAIN = f"{DOCKER_USER}-django-girls-valencia.onrender.com"

ALLOWED_HOSTS = [DOMAIN, "localhost", "127.0.0.1"]

CSRF_TRUSTED_ORIGINS = [f"https://{DOMAIN}"]
```

Usaremos docker para empaquetar todo el contenido de tu web, al igual que antes reemplazada DOCKER_USER con tu nombre de usuario.

Al ejecutar los siguientes comando se te solicitará usuario y contraseña de tu cuenta de dockerhub.
```bash
DOCKER_USER=adalovelace
docker build --no-cache -t $DOCKER_USER/django-girls-valencia:latest .
docker push $DOCKER_USER/django-girls-valencia:latest
```

Una vez publicado, crea un proyecto en render. El nombre del proyecto debe ser "NombreDeUsuario Django Girls Valencia", en el caso de ejemplo sería:

Adalovelace Django Girls Valencia

Selecciona el despliegue del proyecto con una imagen existente de docker, el nombre de la imagen tiene el siguiente formato NombreDeUsuario/django-girls-valencia:latest. En el caso de ejemplo sería:

adalovelace/django-girls-valencia:latest

Selecciona la región Frankfurt (EU Central) y el plan gratuito.

Haz click en Desplegar el servicio, espera unos minutos y... ¡la web ya estará disponible!

https://adalovelace-django-girls-valencia.onrender.com/articles

Al igual que en los casos anteriores reemplaza tu nombre en la url.

- Preguntas que puedes hacer al mentor: ¿Existen alternativas a docker y render para publicar la web?

## Expandiendo tu blog

Habla con tu mentora sobre cómo puedes expandir tu blog. Aquí tienes algunas ideas:

- Usar un sistema de plantillas para no repetir código HTML.
- Añadir un sistema de likes.
- Añadir un sistema de comentarios.
- Incorporar un buscador.
- [Crea un chat en tiempo real](https://programadorwebvalencia.com/django-chat-usando-websockets-con-salas-y-async/).
