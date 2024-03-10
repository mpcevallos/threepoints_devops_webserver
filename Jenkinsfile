@Library('threepoints-sharedlib@main') _

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Especifica la URL del repositorio al clonar
                    def branchName = sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true, 
                                        displayName: 'Get Branch Name', 
                                        env: [GIT_TERMINAL_PROMPT: '0', GIT_ASKPASS: 'echo', GIT_SSH_COMMAND: 'ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no'])?.trim()
                    
                    echo "branchName: ${branchName}"
                }
            }
        }

        stage('Pruebas de SAST') {
            steps {
                script {
                    // Llamada a la función desde la biblioteca compartida
                    sonarAnalysis(abortPipeline: false, branchName: 'master')
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
