pipeline {
    agent { label 'worker' }

    environment {
        DOCKER_IMAGE = 'vivekdalsaniya/notes-app:latest'
        DOCKERHUB_CREDS = credentials('dockercred')  // Your DockerHub creds ID in Jenkins
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
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin"
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Deploy to Server') {
            steps {
                script {
                    // Adjust path and commands based on your server setup
                    sh """
                    docker pull $DOCKER_IMAGE
                    docker stop notes-app || true
                    docker rm notes-app || true
                    docker run -d --name notes-app -p 8000:8000 notes-app
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
