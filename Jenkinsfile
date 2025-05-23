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

        stage('Run Tests') {
            steps {
                script {
                    def testJob = build job: 'TAQC-Team 2', wait: true

                    copyArtifacts(
                        projectName: 'TAQC-Team 2',
                        filter: 'results.xml',
                        selector: specific("${testJob.getNumber()}")
                    )
                }
            }
        }

        stage('Publicar Resultado') {
            steps {
                script {
                    def testFailed = false
                    def resultXml = readFile 'results.xml'
                    if (resultXml.contains('failures="0"') && resultXml.contains('errors="0"')) {
                        testFailed = false
                    } else {
                        testFailed = true
                    }

                    def conclusion = testFailed ? 'FAILURE' : 'SUCCESS'

                    publishChecks(
                        name: 'Automated-tests',
                        title: 'Test Result',
                        summary: conclusion == 'SUCCESS' ? '✅ Todos los tests pasaron.' : '❌ Algunos tests fallaron.',
                        conclusion: conclusion
                    )
                }
            }
        }
    }
}
