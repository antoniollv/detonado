pipeline {
    agent any
    parameters {
        // El token se recibe como parámetro para asegurar que se use el mismo en el callback.
        string(name: 'WEBHOOK_TOKEN', defaultValue: '', description: 'Token para el callback')
    }
    environment {
        // Definir la URL del callback: debe coincidir con el endpoint configurado en el primer pipeline.
        // Aquí se asume que el endpoint es el expuesto por el plugin Webhook Step.
        CALLBACK_URL = "https://jenkins.moradores.es/webhook?token=${params.WEBHOOK_TOKEN}"
    }
    stages {
        stage('Ejecución del Segundo Pipeline') {
            steps {
                echo "Ejecutando el segundo pipeline..."
                // Simulación de trabajo
                sleep time: 30, unit: 'SECONDS'
            }
        }
    }
    post {
        always {
            script {
                def durationSec = currentBuild.duration ? currentBuild.duration / 1000 : 'N/A'
                def jobInfo = [
                    jobNumber: currentBuild.number,
                    repo: env.JOB_NAME,
                    duration: durationSec,
                    status: currentBuild.currentResult
                ]
                def jsonJobInfo = groovy.json.JsonOutput.toJson(jobInfo)
                echo "Enviando callback al endpoint: ${env.CALLBACK_URL}"
                
                // Se envía el callback usando httpRequest (asegúrate de tener instalado el plugin HTTP Request)
                httpRequest(
                    httpMode: 'POST',
                    url: env.CALLBACK_URL,
                    requestBody: jsonJobInfo,
                    contentType: 'APPLICATION_JSON',
                    validResponseCodes: '200,201,202,204',
                    authentication: 'jenkins-api-credentials2'
                )
            }
        }
    }
}

