@Library('threepoints-sharedlib@main') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
        steps {
            script {
                def branchName = env.BRANCH_NAME
                echo "Checking out branch: ${branchName}"
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
