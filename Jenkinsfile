pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'vivekdalsaniya/notes-app'
        DOCKERHUB_CREDS = credentials('dockercreds')  // Your DockerHub creds ID in Jenkins
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/vivekdalsaniya12/django-notes-app.git'
            }
        }

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
                    '''
                }
            }
        }


        stage('Deploy to Server') {
            steps {
                script {
                    // Adjust path and commands based on your server setup
                    sh """
                    docker pull $DOCKER_IMAGE:latest
                    docker stop $DOCKER_IMAGE || true
                    docker rm $DOCKER_IMAGE || true
                    docker run -d --name $DOCKER_IMAGE -p 8000:8000 $DOCKER_IMAGE:latest
                    """
                }
            }
        }
    }

    post {
        failure {
            echo 'Build failed. Check the logs.'
        }
        success {
            echo ' Build and deployment successful.'
        }
    }
}
