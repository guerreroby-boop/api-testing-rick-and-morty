pipeline {
    agent any
    
    tools {
        nodejs 'node20' 
    }

    environment {
        // Asegúrate de que el ID de la credencial sea exacto
        P_KEY = credentials('POSTMAN_API_KEY')
        
        // REVISA ESTOS IDs en la web de Postman (Info del elemento)
        COLL_ID = 'TU_ID_DE_COLECCION_AQUI'
        ENV_ID = 'TU_ID_DE_ENTORNO_AQUI'
        
        // Construcción de URLs con comillas DOBLES
        COLLECTION_URL = "https://api.postman.com/collections/${COLL_ID}?access_key=${P_KEY}"
        ENV_URL = "https://api.postman.com/environments/${ENV_ID}?access_key=${P_KEY}"
    }

    stages {
        stage('Preparar Ambiente') {
            steps {
                sh 'npm install'
            }
        }

        stage('Ejecutar Pruebas de API') {
            steps {
                // Forzamos el uso de las variables directamente en el comando
                sh "newman run \"${COLLECTION_URL}\" -e \"${ENV_URL}\" -r cli,htmlextra --reporter-htmlextra-export report.html"
            }
        }
        stage('Debug Secret') {
        steps {
            // Esto imprimirá asteriscos si la credencial existe
            sh 'echo "La API Key configurada es: $P_KEY"'
            }
        }
    }
    
    post {
        always {
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