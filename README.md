# Jenkins Docker

Proyecto académico para practicar integración continua y despliegue utilizando Jenkins y Docker.

## Archivos principales

- `index.html`: página web de prueba.
- `Dockerfile`: configuración para crear la imagen Docker.
- `Jenkinsfile`: definición del Pipeline de Jenkins.
- `README.md`: documentación del proyecto.

## Tecnologías utilizadas

- Jenkins
- Docker
- Git
- GitHub
- Nginx
- HTML

## Verificación local con Docker

Antes de configurar Jenkins, se realizó una prueba local de la aplicación utilizando Docker.

### 1. Construcción de la imagen

Se construyó la imagen Docker con el nombre `jenkins-docker-botzoc` y la versión `v1.0`.

```bash
docker build -t jenkins-docker-botzoc:v1.0 .