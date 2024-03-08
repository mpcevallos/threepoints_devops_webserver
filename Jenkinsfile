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
                    sh '${SONAR_RUNNER_HOME}/bin/sonar-scanner -Dsonar.host.url=http://10.0.2.15:9000/ -Dsonar.login=squ_486714c6dafa6ce689d33b560ba42ccb6b6e2037 -Dsonar.projectKey=sonarqube -Dsonar.sources=. -Dsonar.tests=. -Dsonar.exclusions=**/node_modules/** -Dsonar.coverage.exclusions=**/node_modules/**'
                }
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t threepoints_devops_webserver .'
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

