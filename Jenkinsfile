pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git']]])
            }
        }

        stage('Pruebas de SAST') {
            steps {
                script {
                    echo 'Ejecución de pruebas de SAST'
                }
            }
        }

        stage('Configurar archivo') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'git-threepoints-github', keyFileVariable: 'KEY_FILE', passphraseVariable: '', usernameVariable: 'USERNAME')]) {
                        sh 'echo "[credentials]" > credentials.ini'
                        sh 'echo "user=${USERNAME}" >> credentials.ini'
                        sh 'echo "password=${KEY_FILE}" >> credentials.ini'
                    }
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
