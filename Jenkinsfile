@Library('threepoints-sharedlib') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git'
            }
        }

       stage('Pruebas de SAST') {
    steps {
        script {
            // Utilizamos la funcion sonarAnalysis
            scriptLibs.sonarAnalysis(abortPipeline: false)
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

