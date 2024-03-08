pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: '*/master', credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv() {
                    sh '${SONAR_RUNNER_HOME}/bin/sonar-scanner -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_LOGIN} -Dsonar.projectKey=${PROJECT_KEY} -Dsonar.sources=. -Dsonar.tests=. -Dsonar.projectVersion=${VERSION}'
                }
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${VERSION} .'
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

