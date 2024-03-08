pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/threepoints_devops_webserver.git'
            }
        }

       stage('SonarQube Analysis') {
    steps {
        script {
            def scannerHome = tool 'sonar-scanner'
            withEnv(["PATH+SONAR=${scannerHome}/bin"]) {
                sh """
                    sonar-scanner \
                    -Dsonar.projectKey=sonarqube \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.login=sqp_8871e861546564ca35025574380ccc281e056c0c
                """
            }
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

