@Library('threepoints-sharedlib@main') _

import com.threepoints.sharedlib.ScriptLibs

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
                    // Importamos la biblioteca
                    def scriptLibs = new ScriptLibs()
                    // Utilizamos la función sonarAnalysis
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
