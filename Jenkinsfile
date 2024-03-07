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
                    // Ejecutar SonarQubeScan
                    sonarqube(
                        credentialsId: 'sonar-token', // ID de las credenciales de SonarQube
                        installationName: 'SonarQube', // Nombre de la instalación de SonarQube en Jenkins
                        projectKey: 'sast-sonarqube', // Clave del proyecto en SonarQube
                        projectName: 'SonarQube SAST', // Nombre del proyecto en SonarQube
                        scannerHome: '${scannerHome}/bin/sonar-scanner' // Ruta al directorio del escáner de SonarQube
                    )
                    
                    // Ejecutar Quality Gate 
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: false
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
