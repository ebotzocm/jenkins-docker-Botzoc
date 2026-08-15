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
                echo 'Repositorio obtenido correctamente desde GitHub'
                sh 'git status'
            }
        }

        stage('Verificacion') {
            steps {
                echo 'Verificando archivos necesarios...'

                sh '''
                    if [ ! -f index.html ]; then
                        echo "ERROR: No se encontro index.html"
                        exit 1
                    fi

                    if [ ! -f Dockerfile ]; then
                        echo "ERROR: No se encontro Dockerfile"
                        exit 1
                    fi

                    echo "index.html encontrado"
                    echo "Dockerfile encontrado"
                '''
            }
        }

        stage('Construccion') {
            steps {
                echo "Construyendo imagen Docker version ${BUILD_NUMBER}"

                sh '''
                    docker build \
                    -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                '''
            }
        }

        stage('Despliegue') {
            steps {
                echo 'Eliminando contenedor anterior si existe...'

                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    echo "Creando nuevo contenedor..."

                    docker run -d \
                    --name ${CONTAINER_NAME} \
                    -p ${APP_PORT}:80 \
                    ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Confirmacion') {
            steps {
                echo 'Verificando contenedor desplegado...'

                sh '''
                    docker ps --filter "name=${CONTAINER_NAME}"
                '''

                echo "Aplicacion desplegada correctamente"
                echo "Imagen: ${IMAGE_NAME}:${BUILD_NUMBER}"
                echo "Puerto publicado: ${APP_PORT}"
            }
        }
    }

    post {

        success {
            echo '============================================'
            echo 'PIPELINE EJECUTADO EXITOSAMENTE'
            echo 'Aplicacion desplegada con Jenkins y Docker'
            echo '============================================'
        }

        failure {
            echo '============================================'
            echo 'ERROR: EL PIPELINE HA FALLADO'
            echo 'Revise la salida de consola de Jenkins'
            echo '============================================'
        }
    }
}