pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Obteniendo codigo del repositorio...'
            }
        }

        stage('Build') {
            steps {
                echo 'Construyendo imagen Docker...'
            }
        }

        stage('Test') {
            steps {
                echo 'Ejecutando pruebas...'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Desplegando aplicacion...'
            }
        }
    }
}