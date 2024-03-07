pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Pruebas de SAST') {

            steps {
                script {
                    // Ejecutar SonarQubeScan
                    sonarqube(
                        credentialsId: 'sonar-token', 
                        installationName: 'SonarScanner', 
                        projectKey: 'node-app-sonarqubescan',
                        projectName: 'App Node', 
                        scannerHome: '${scannerHome}/bin/sonar-scanner'
                    )
                    
                   timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: false
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
