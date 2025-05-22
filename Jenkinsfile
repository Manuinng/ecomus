pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Trigger Test ') {
            steps {
                echo 'Triggering test job...'
            }
        }
    }
}