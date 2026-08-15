pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-docker-botzoc'
        CONTAINER_NAME = 'web-jenkins-botzoc'
        APP_PORT = '8081'
    }

    stages {

        stage('Clonacion') {
            steps {
                echo 'Obteniendo repositorio desde GitHub...'
                checkout scm
                echo 'Repositorio clonado correctamente.'
            }
        }

        stage('Verificacion') {
            steps {
                echo 'Verificando archivos obligatorios...'

                sh '''
                    test -f index.html || {
                        echo "ERROR: No se encontro index.html"
                        exit 1
                    }

                    test -f Dockerfile || {
                        echo "ERROR: No se encontro Dockerfile"
                        exit 1
                    }

                    echo "index.html encontrado"
                    echo "Dockerfile encontrado"
                '''
            }
        }

        stage('Construccion') {
            steps {
                echo "Construyendo imagen Docker ${IMAGE_NAME}:${BUILD_NUMBER}"

                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Despliegue') {
            steps {
                echo 'Reemplazando contenedor anterior si existe...'

                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p ${APP_PORT}:80 \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Confirmacion') {
            steps {
                echo 'Comprobando despliegue...'

                sh '''
                    sleep 2

                    docker ps --filter "name=${CONTAINER_NAME}"

                    curl -f http://host.docker.internal:${APP_PORT} > /dev/null
                '''

                echo "Aplicacion disponible en el puerto ${APP_PORT}"
            }
        }
    }

    post {
        success {
            echo '=========================================='
            echo 'PIPELINE EJECUTADO EXITOSAMENTE'
            echo 'Aplicacion desplegada con Jenkins y Docker'
            echo '=========================================='
        }

        failure {
            echo '=========================================='
            echo 'ERROR: EL PIPELINE HA FALLADO'
            echo 'Revise la salida de consola de Jenkins'
            echo '=========================================='
        }
    }
}