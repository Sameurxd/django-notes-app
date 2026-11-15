pipeline {
    agent { label 'sameur' }

    environment {
        IMAGE_NAME = "imsameur/django-note-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
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
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                        -u "$DOCKER_USER" --password-stdin

                        docker push $IMAGE

                        docker logout
                      '''  
                }

                echo 'push completed successfully'
            }
        }

        stage('Deploy on Agent Sameur'){
            steps{
                echo 'Deploying image to agent started'
                sh '''
                    cd /root/django-notes-app
                    docker compose up -d --no-build
                    curl -fsS http://127.0.0.1:8000/api/notes/ || exit 1
                '''

                echo 'Deployment completed successfully'                
            }

        }
        
    }
}
