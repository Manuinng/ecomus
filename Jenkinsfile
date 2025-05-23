pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Trigger Test Job') {
            steps {
                script {
                    // Obtener el SHA del commit actual
                    def commitSha = sh(script: "git rev-parse HEAD", returnStdout: true).trim()
                    echo "Commit SHA: ${commitSha}"

                    // Llamar al job 2, pasándole el commit SHA como parámetro
                    build job: 'TAQC-Team 2', parameters: [
                        string(name: 'COMMIT_SHA', value: commitSha)
                    ]
                }
            }
        }
    }
}