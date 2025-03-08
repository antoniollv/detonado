pipeline {
    agent any
    stages {
        stage('Ejecución del Segundo Pipeline') {
            steps {
                echo "Iniciando ejecución en el segundo repositorio..."
                // Simula trabajo (por ejemplo, compilar, testear, etc.)
                sleep time: 30, unit: 'SECONDS'
            }
        }
    }
    post {
        always {
            script {
                // Calcula el tiempo empleado en segundos (currentBuild.duration está en milisegundos)
                def durationSec = currentBuild.duration ? currentBuild.duration / 1000 : "N/A"
                echo "Resumen del Segundo Pipeline:"
                echo "Job Number: ${currentBuild.number}, Repo: ${env.JOB_NAME}, Duration: ${durationSec} seg, Status: ${currentBuild.currentResult}"
            }
        }
    }
}
