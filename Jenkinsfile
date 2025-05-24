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
                    testBuild = build job: 'TAQC-Team 2', propagate: false, wait: true
                }

                copyArtifacts(
                    projectName: 'TAQC-Team 2',
                    selector: specific("${testBuild.number}"),
                    filter: 'results.xml',
                    target: 'jobs-results',
                    flatten: true
                ) 
            }
        }

        stage('Publicar Resultado de Tests') {
            steps {
                publishChecks name: 'Resultados de Tests',
                              results: [junit: 'jobs-results/results.xml']
            }
        }
    }
}
