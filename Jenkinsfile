pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'vivekdalsaniya/notes-app'
        CONTAINER_NAME = 'notes-app' //  valid container name without slash
        DOCKERHUB_CREDS = credentials('dockercreds')
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh "whoami"
                    sh "docker build -t $DOCKER_IMAGE:latest ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockercreds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    sh """
                        docker compose up -d
                    """
                }
            }
        }
    }

    post {
        failure {
            echo ' Build failed. Check the logs.'
        }
        success {
            echo ' Build and deployment successful.'
        }
    }
}
