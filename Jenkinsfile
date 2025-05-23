pipeline {
    agent any

    environment {
        GITHUB_APP = credentials('github-checks-app')
        REPO = 'Manuinng/ecomus'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Trigger Test Job') {
            steps {
                script {
                    def commitSha = sh(script: "git rev-parse HEAD", returnStdout: true).trim()
                    build job: 'TAQC-Team 2', parameters: [string(name: 'GIT_COMMIT_SHA', value: commitSha)]
                }
            }
        }
    }
}
