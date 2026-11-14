pipeline {
    agent { label 'sameur' }

    stages {
        
        stage('Code Clone from Github') {
            steps {
                sh 'Cloning code from github'
                git branch: 'main',
                url: 'https://github.com/Sameurxd/django-notes-app.git',
                sh 'Code clone completed successfully.'
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
