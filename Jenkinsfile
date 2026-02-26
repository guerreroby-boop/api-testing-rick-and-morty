pipeline {
    agent any

    environment {
        // Estas credenciales las debes crear en Jenkins como 'Secret Text'
        POSTMAN_API_KEY = credentials('POSTMAN_API_KEY')
        
        // Reemplaza estos IDs con los que obtuviste de Postman
        COLLECTION_ID = 'tu-id-de-coleccion'
        ENV_ID = 'tu-id-de-entorno'
        
        // Construimos las URLs dinámicamente
        COLLECTION_URL = "https://api.postman.com/collections/${COLLECTION_ID}?access_key=${POSTMAN_API_KEY}"
        ENV_URL = "https://api.postman.com/environments/${ENV_ID}?access_key=${POSTMAN_API_KEY}"
    }

    stages {
        stage('Preparar Ambiente') {
            steps {
                // Instalamos las dependencias del package.json (newman y htmlextra)
                sh 'npm install'
            }
        }

        stage('Ejecutar Pruebas de API') {
            steps {
                // Ejecutamos el script 'test' que definimos en el package.json
                // Le pasamos las URLs como variables de entorno
                sh 'npm test'
            }
        }
    }

    post {
        always {
            // Publicamos el reporte htmlextra en la interfaz de Jenkins
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.', 
                reportFiles: 'report.html',
                reportName: 'Reporte de APIs Rick & Morty'
            ])
        }
    }
}