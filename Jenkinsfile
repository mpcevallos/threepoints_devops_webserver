pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Pruebas de SAST') {
            steps {
                script {
                    echo 'Ejecución de pruebas de SAST'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'docker build -t devops_threepoints .'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline ejecutado exitosamente'
        }

        failure {
            echo 'Pipeline ha fallado'
        }
    }
}
