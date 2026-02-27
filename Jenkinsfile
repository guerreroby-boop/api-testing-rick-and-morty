pipeline {
    agent any
    
    tools {
        nodejs 'node20' 
    }

    environment {
        // Asegúrate de que el ID de la credencial sea exacto
        P_KEY = credentials('POSTMAN_API_KEY')
        
        // REVISA ESTOS IDs en la web de Postman (Info del elemento)
        COLL_ID = '52350672-de8ed765-9857-43a4-8ac1-a3ae1bb9c4b4'
        ENV_ID = '52350672-97baea4a-58eb-49a3-b269-347099adf81d'
        
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
            sh "newman run https://api.postman.com/collections/${COLL_ID} -H 'X-Api-Key: ${P_KEY}' -e https://api.postman.com/environments/${ENV_ID} -H 'X-Api-Key: ${P_KEY}' -r cli,htmlextra --reporter-htmlextra-export report.html"
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