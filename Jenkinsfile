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

        stage('Run Tests Job') {
            steps {
                script {
                    def buildResult = build job: 'TAQC-Team 2', parameters: [
                        string(name: 'COMMIT_SHA', value: env.GIT_COMMIT)
                    ], wait: true

                    echo "TAQC-Team 2 finished with result: ${buildResult.getResult()}"
                }
            }
        }

        stage('Publish GitHub Check') {
            steps {
                script {
                    def conclusion = (currentBuild.result == 'SUCCESS') ? 'SUCCESS' : 'FAILURE'

                    publishChecks(
                        name: 'Automated-tests',
                        conclusion: conclusion,
                        title: 'Test Result',
                        summary: (conclusion == 'SUCCESS') ? '✅ All tests passed.' : '❌ Some tests failed.'
                    )
                }
            }
        }
    }
}