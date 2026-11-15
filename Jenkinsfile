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

        stage('Deploy on Agent Sameur') {
            steps {
                echo 'Deploying image to agent started'
                sh '''
                    set -e
                    cd /opt/django-notes-app
                    export IMAGE_TAG=$IMAGE_TAG
                    C="docker compose -f docker-compose.yml -f docker-compose.deploy.yml"

                    echo "Deploying tag $IMAGE_TAG from Docker Hub"
                    $C pull
                    $C up -d --no-build
                    $C ps

                    for i in $(seq 1 20); do
                        if curl -fsS --max-time 3 http://127.0.0.1:8000/api/notes/ >/dev/null; then
                            echo "Smoke test OK - running tag $IMAGE_TAG"; exit 0
                        fi
                        echo "waiting for app... ($i/20)"; sleep 3
                    done
                    $C logs --tail 50 django_app
                    exit 1
                '''
                echo 'Deployment completed successfully'
            }
        }
        }
        
    }
}
