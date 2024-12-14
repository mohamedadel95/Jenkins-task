pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'mohamedadel95/node-app'
        DOCKER_TAG = 'latest'
    }
    stages {
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 3000:3000 $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    // Log in to DockerHub
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    
                    // Push the image to DockerHub
                    sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
                }
            }
        }
    }
    post {
        always {
            // Clean up by stopping and removing the Docker container
            sh 'docker stop $(docker ps -q --filter ancestor=$DOCKER_IMAGE:$DOCKER_TAG)'
            sh 'docker rm $(docker ps -a -q --filter ancestor=$DOCKER_IMAGE:$DOCKER_TAG)'
        }
    }
}
