@Library('threepoints-sharedlib@main') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Accede al nombre de la rama directamente desde la variable de entorno
                    def branchName = env.BRANCH_NAME
                    echo "branchName: ${branchName}"
                }
            }
        }

        stage('Pruebas de SAST') {
            steps {
                script {
                    // Llamada a la función desde la biblioteca compartida
                    sonarAnalysis(abortPipeline: false, branchName: env.BRANCH_NAME)
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t devops_threepoints:latest .'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
