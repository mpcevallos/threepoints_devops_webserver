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
            // Configurar el entorno de SonarQube
            withSonarQubeEnv('SonarScanner') {
                // Ejecutar SonarQubeScan
                sh "${scannerHome}/bin/sonar-scanner"
                
                // Ejecutar Quality Gate 
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
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
