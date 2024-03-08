pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']],
                          userRemoteConfigs: [[credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git']])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv() {
                        sh ''' ${scannerHome}/bin/sonar-scanner" -Dsonar.url=http://10.0.2.15:9000/
 -Dsonar.login=squ_486714c6dafa6ce689d33b560ba42ccb6b6e2037 -Dsonar.projectName=threepoints_devops_webserver -Dsonar.projectKey=sonarqube -Dsonar.sources=. -Dsonar.tests=. -Dsonar.exclusions=**/node_modules/** -Dsonar.coverage.exclusions=**/node_modules/**'''
                    }
                }
                // Ejecutar Quality Gate 
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
