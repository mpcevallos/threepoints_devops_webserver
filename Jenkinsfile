pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Primer Stage que descarga el código de GitHub
                checkout scm
            }
        }
        
        stage('Pruebas de SAST') {
            steps {
                // Segundo Stage que realizado prueba de analisis de código estático simulado
                echo 'Ejecución de pruebas de SAST'
            }
        }
        
        stage('Build') {
            steps {
                // Tercer Stage que construye el contenedor de Docker
                script {
                    docker.build('node_devops')
                }
            }
        }
    }
}
