pipeline {
    agent any
    parameters {
        string(name: 'CALLBACK_URL', description: 'Call back URL')
    }
    stages {
        stage('Ejecución del Segundo Pipeline') {
            steps {
                echo 'Ejecutando pipeline...'
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
                def secureURL = params.CALLBACK_URL.replaceFirst('^http://', 'https://')
                echo "Enviando callback al endpoint: ${secureURL}"
                httpRequest(
                    httpMode: 'POST',
                    url: secureURL,
                    requestBody: jsonJobInfo,
                    contentType: 'APPLICATION_JSON',
                    validResponseCodes: '200,201,202,204',
                    authentication: 'jenkins-api-credentials2'
                )
            }
        }
    }
}
