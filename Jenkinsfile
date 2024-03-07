pipeline {
    agent {
        docker {
            image 'node:10-alpine'
        }
    }

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
        
    }
}

