# Construcción del proyecto usando Podman

¿Qué es Podman? Podman es la alternativa de código abierto para usar un motor de contenedores en lugar de Docker. Otra característica definitoria de Podman es que no necesita un demonio. Un demonio es un programa que se ejecuta en segundo plano para manejar servicios, procesos y solicitudes sin una interfaz de usuario. Es una visión única del motor de contenedores, ya que no depende de un demonio, sino que lanza contenedores y pods como procesos hijos.
Requisitos

Estos pasos fueron validados en Fedora (una de las muchas distribuciones de Linux), y se espera que funcionen en otras como Ubuntu/Debian o macOS.

Instalar los siguientes paquetes
- `podman` segun [documentacion](https://podman.io/docs/installation)
- `podman-compose`
```
# Debian/Ubuntu
sudo apt-get install podman-compose -y
# Fedora
sudo dnf update install podman-compose -y
```
## Empezemos
Crea una carpeta donde se quedara todos los archivos.
Empezemos ahora con dos archivos. El `Containerfile` y `compose.yml`

1. Crea un archivo llamado `Containerfile`
```
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

# Expose port
EXPOSE 10000

CMD ["python", "manage.py", "runserver", "0.0.0.0:10000"]
```
2. Crea un archivo llamado `compose.yml`.

*Curiosidad: La principal diferencia entre este archivo y el de las instrucciones originales es que se abordan problemas de permisos en el volumen con`:Z`*
```
services:
  django:
    build:
      context: .
      dockerfile: Containerfile 
    volumes:
      - .:/usr/src/app/:Z
    ports:
    - "10000:10000"
```
### Creando un proyecto Django
```
podman-compose run django django-admin startproject my_app .
podman-compose run django python manage.py startapp my_blog
```
### Levantar un servidor
```
podman-compose server up
```
---

Aquí solo estoy proporcionando los archivos principales para empezar.
A partir de aquí, continuar con las instrucciones originales debería ser suficiente desde el paso ["Creando una aplication"](https://github.com/ehvs/django-girls-valencia?tab=readme-ov-file#creando-una-aplicaci%C3%B3n)

⚠️ **No olvides**, siempre que veas `docker compose` en las instrucciones originales, reemplázalo por `podman-compose`.
⚠️ **Attencion**: Siempre que sea necesario reconstruir el servidor para que los cambios surtan efecto, ejecuta lo siguiente:
```
# primero
podman-compose server stop
# segundo
podman-compose server up --rebuild
```

