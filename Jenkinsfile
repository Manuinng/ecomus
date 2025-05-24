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

        stage('Publish Test Results') {
            steps {
                junit 'jobB-results/results.xml'

                publishChecks name: 'Test Results',
                              summary: 'Integration tests were executed.',
                              title: 'Test Results',
                              status: 'COMPLETED',
                              conclusion: 'SUCCESS'
            }
        }
    }
}
