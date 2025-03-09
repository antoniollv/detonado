pipeline {
    agent any
    environment {
        // URL del callback: se usa la URL interna de Jenkins para disparar el job del primer repositorio.
        // Asegúrate de que el job del primer repositorio tenga configurado el token 'myToken'
        CALLBACK_URL = "https://jenkins.moradores.es/job/Detonador/job/detonador/job/develop/build?token=myToken"
    }
    stages {
        stage('Ejecución del Segundo Pipeline') {
            steps {
                echo "Ejecutando Segundo Pipeline..."
                // Simulación de trabajo (por ejemplo, compilación, pruebas, etc.)
                sleep time: 30, unit: 'SECONDS'
            }
        }
    }
    post {
        always {
            script {
                def durationSec = currentBuild.duration ? currentBuild.duration / 1000 : 'N/A'
                // Se arma un mapa con la información a enviar
                def jobInfo = [
                    jobNumber: currentBuild.number,
                    repo: env.JOB_NAME,
                    duration: durationSec,
                    status: currentBuild.currentResult
                ]
                // Convertir el mapa a JSON
                def jsonJobInfo = groovy.json.JsonOutput.toJson(jobInfo)
                echo "Enviando hook callback: ${jsonJobInfo}"
                // Se envía la notificación (hook) mediante una petición HTTP POST
                httpRequest(
                    httpMode: 'POST',
                    url: env.CALLBACK_URL,
                    requestBody: jsonJobInfo,
                    contentType: 'APPLICATION_JSON',
                    validResponseCodes: '200'
                )
            }
        }
    }
}
