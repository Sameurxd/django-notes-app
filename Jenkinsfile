pipeline {
    agent { label 'sameur' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Test') {
            steps {
                sh 'echo Hello from Jenkins agent'
                sh 'whoami'
            }
        }
    }
}
