pipeline {
    agent any

    environment {
        SONAR_RUNNER_HOME = tool 'SonarScanner'
        SONAR_HOST_URL = 'http://10.0.2.15:9000/'
        SONAR_LOGIN = 'squ_486714c6dafa6ce689d33b560ba42ccb6b6e2037'
        PROJECT_KEY = 'sonarqube'
        VERSION = '1.0'
        IMAGE_NAME = 'devops_threepoints'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarScanner') {
                    sh "${SONAR_RUNNER_HOME}/bin/sonar-scanner -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_LOGIN} -Dsonar.projectKey=${PROJECT_KEY} -Dsonar.sources=. -Dsonar.tests=. -Dsonar.projectVersion=${VERSION}"
                }
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${VERSION} ."
                }
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
