pipeline {
    agent any
    parameters {
        // El token se recibe como parámetro para asegurar que se use el mismo en el callback.
        string(name: 'CALLBACK_URL', description: 'Call back URL')
    }
    
    stages {
        stage('Ejecución del Segundo Pipeline') {
            steps {
                echo "Ejecutando el segundo pipeline..."
                // Simulación de trabajo
                sleep time: 3, unit: 'SECONDS'
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
                echo "Enviando callback al endpoint: ${params.CALLBACK_URL}"
                
                // Se envía el callback usando httpRequest 
                httpRequest(
                    httpMode: 'POST',
                    url: params.CALLBACK_URL,
                    requestBody: jsonJobInfo,
                    contentType: 'APPLICATION_JSON',
                    validResponseCodes: '200,201,202,204',
                    authentication: 'jenkins-api-credentials2'
                )
            }
        }
    }
}

