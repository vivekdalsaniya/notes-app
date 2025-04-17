pipeline {
    agent { label 'worker' }
    environment {
        MY_CRED = credentials('dockercreds') 
    }
    stages {
        stage('git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/vivekdalsaniya12/django-notes-app.git'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t vivekdalsaniya/django-notes-app:latest .'
            }
        }
        stage('docker image push'){
            steps{
                sh '''
                    echo $MY_CRED_PSW | docker login -u $MY_CRED_USR --password-stdin
                    docker push vivekdalsaniya/django-notes-app:latest
                '''
            }
        }
    }
}
