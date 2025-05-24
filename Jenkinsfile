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
                        target: 'jobs-results',
                        flatten: true
                    )
                    env.TEST_BUILD_NUMBER = testBuildNumber
                }

            }
        }

        stage('Publish Test Results') {
            steps {
                def testBuildNumber = env.TEST_BUILD_NUMBER

                junit "jobs-results/results-${testBuildNumber}.xml"

                publishChecks name: 'Test Results',
                              summary: 'Integration tests were executed.',
                              title: 'Test Results',
                              status: 'COMPLETED',
                              conclusion: currentBuild.currentResult == 'SUCCESS' ? 'SUCCESS' : 'FAILURE',
            }
        }
    }
}
