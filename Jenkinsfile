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
                    def testBuild = build job: 'TAQC-Team 2', propagate: false, wait: true
                    env.TEST_BUILD_NUMBER = testBuild.number.toString()

                    copyArtifacts(
                        projectName: 'TAQC-Team 2',
                        selector: specific(env.TEST_BUILD_NUMBER),
                        filter: "results-${env.TEST_BUILD_NUMBER}.xml",
                        target: 'Report',
                        flatten: true
                    )
                }

            }
        }

        stage('Publish Test Results') {
            steps {
                junit "Report/results-${env.TEST_BUILD_NUMBER}.xml"
            }
        }
    }
}
