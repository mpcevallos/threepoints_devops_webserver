pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                // Realiza un checkout del repositorio GitHub
                checkout scm
            }
        }

        stage('Pruebas de SAST') {
            steps {
                // Ejecuta pruebas de SAST simulada con echo
                script {
                    echo 'Ejecución de pruebas de SAST'
                }
            }
        }

        stage('Configurar archivo') {
            steps {
                // Crea un archivo credentials.ini con las credenciales de GitHub y lo archiva como artefacto
                script {
                    // Utiliza withCredentials para manejar las credenciales
                    withCredentials([usernamePassword(credentialsId: 'git-threepoints-github', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        // Crea el archivo credentials.ini
                        writeFile file: 'credentials.ini', text: "[credentials]\nuser=${USERNAME}\npassword=${PASSWORD}"
                    }

                    // Archiva el archivo como artefacto
                    archiveArtifacts artifacts: 'credentials.ini', onlyIfSuccessful: true
                }
            }
        }

        stage('Build') {
            // Construye la imagen de Docker con el archivo credentials.ini archivado como artefacto
            steps {
                script {
                    sh 'docker build -t devops_threepoints .'
                }
            }
        }
    }

    post {
        // Resultado de pipeline exitoso
        success {
            echo 'Pipeline ejecutado exitosamente'
        }
        // Resultado de pipeline fallido
        failure {
            echo 'Pipeline ha fallado'
        }
    }
}
