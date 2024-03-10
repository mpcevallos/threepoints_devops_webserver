@Library('threepoints-sharedlib@main') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
        steps {
            script {
                // Obtiene el nombre de la rama actual de Git
                def branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                echo "branchName: ${branchName}"
                git branch: branchName, credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git'
            }
        }
    }

        stage('Pruebas de SAST') {
            steps {
                script {
                    // Llamada a la función desde la biblioteca compartida
                    sonarAnalysis(abortPipeline: false, branchName: env.master)
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
