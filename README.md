1. Automatización de infraestructura 
Para automatizar el proyecto se utilizaron workflows de GitHub Actions, los cuales permitieron realizar procesos automáticos de integración y despliegue continuo (CI/CD).
Los workflows fueron configurados para:

construir imágenes Docker,
subir las imágenes a Dockerhub,
y actualizar automáticamente los contenedores en las instancias EC2.

Cada parte del sistema cuenta con su propio workflow:
frontend
backend
base de datos

Los workflows se ejecutan automáticamente cuando se realizan cambios en GitHub mediante un git push.
Además, se utilizaron secrets para almacenar credenciales AWS y datos importantes de forma segura, evitando exponer información sensible dentro del repositorio.
La automatización permitió:
reducir errores manuales,
acelerar los despliegues,
mantener una mejor organización,
y mejorar la continuidad operativa del sistema.

2. Contenerización con Docker 
La aplicación fue contenerizada utilizando Docker con el objetivo de facilitar el despliegue, la administración y la portabilidad de los servicios.
Cada capa de la arquitectura fue separada en su propio contenedor Docker:
Frontend
El frontend corresponde a la interfaz gráfica de la aplicación y se ejecuta en un contenedor independiente.
Características:

utiliza el puerto 80,
permite el acceso desde Internet,
y entrega la interfaz visual al usuario.

Backend
El backend contiene la lógica de negocio y la comunicación con la base de datos.
Características:
utiliza el puerto 3001,
procesa las solicitudes del frontend,
y administra la información del sistema.

Base de datos
La base de datos se ejecuta en un contenedor MySQL independiente.
Características:
utiliza el puerto 3306,
almacena la información de productos,
y mantiene persistencia mediante volúmenes Docker.

Docker permitió:
separar responsabilidades,
facilitar la administración,
evitar conflictos de dependencias,
y mejorar la portabilidad entre ambientes.

2.1 Docker Compose
Se utilizó Docker Compose para administrar todos los contenedores desde un único archivo de configuración.
A través del archivo docker-compose.yml se configuraron:

frontend,
backend,
base de datos,
redes internas,
puertos,
y volúmenes.

Docker Compose permitió establecer la comunicación entre contenedores de forma automática.
Por ejemplo:
el frontend se comunica con el backend,
y el backend se conecta con la base de datos.

También se utilizó un volumen Docker llamado dbdata para mantener la persistencia de datos de MySQL, evitando la pérdida de información al reiniciar los contenedores.
El uso de Docker Compose ayudó a:
simplificar el despliegue,
mejorar la organización,
y facilitar futuras modificaciones del sistema.

3. Pipeline CI/CD con GitHub Actions 
Se implementó un pipeline CI/CD utilizando GitHub Actions para automatizar el proceso de construcción, publicación y despliegue de la aplicación.
El pipeline realiza automáticamente las siguientes tareas:

descarga el código desde GitHub,
construye la imagen Docker,
pública la imagen en DockerHub,
y despliega automáticamente la nueva versión en EC2.

Cada capa posee su propio workflow:
front-despacho
Back-Ventas_springboot
Back-Despachos_Springboot

Los workflows se ejecutan automáticamente cuando existen cambios en:
frontend
backend
db

Durante el despliegue automático:

la instancia EC2 realiza login en DockerHub,
descarga la nueva imagen,
elimina el contenedor anterior,
y ejecuta el nuevo contenedor actualizado.

Gracias al pipeline CI/CD se logró:

automatizar despliegues,
reducir errores manuales,
mejorar la rapidez de actualización,
y mantener una mejor trazabilidad de cambios.

4.- SCRIPT FRONTEND
	#!/bin/bash
yum update -y
yum upgrade -y
yum install docker -y
yum install git -y
systemctl enable docker
systemctl start docker
usermod -aG docker ec2-user
docker run -d -p 80:80 --name servidor-web nginx

5.-  SCRIPT BACKEND
	#!/bin/bash
yum update -y
yum upgrade -y
yum install docker -y
yum install git -y
systemctl enable docker
systemctl start docker
usermod -aG docker ec2-user
docker run -d -p 8081:8081 --name microservicio-back httpd

6.-SCRIPT DATA
	#!/bin/bash
yum update -y
yum upgrade -y
yum install docker -y
yum install git -y
systemctl enable docker
systemctl start docker
usermod -aG docker ec2-user
docker run -d -p 8082:8082 --name microservicio-back httpd




