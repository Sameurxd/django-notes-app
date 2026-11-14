pipeline {
    agent { label 'sameur' }

    environment {
        IMAGE_NAME = "imsameur/django-note-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE = "${IMAGE_NAME}:${IMAGE}"
    }

    stages {
        
        stage('Code Clone from Github') {
            steps {
                echo 'Cloning code from github'
                git branch: 'main',
                    url: 'https://github.com/Sameurxd/django-notes-app.git'
                echo 'Code clone completed successfully.'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Image'
                sh '''
                    docker build -t $IMAGE .
                    docker tag $IMAGE $IMAGE_NAME:latest
                '''
                echo 'Building completed successfully'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'pushing docker image'
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh '''
                        echo "$PASS" | docker login \
                        -u "$USER" --password-stdin

                        docker push $IMAGE

                        docker logout
                      '''  
                }

                echo 'push completed successfully'
            }
        }


























        
        
    }
}
