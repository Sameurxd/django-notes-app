pipeline {
    agent { label 'sameur' }

    stages {
        
        stage('Code Clone from Github') {
            steps {
                echo 'Cloning code from github'
                git branch: 'main',
                    url: 'https://github.com/Sameurxd/django-notes-app.git'
                echo 'Code clone completed successfully.'
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
