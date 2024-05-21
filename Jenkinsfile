pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials' // Укажите ID ваших учетных данных Docker
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '/usr/local/go/bin/go test .'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("ubuntu-bionic:8082/hello-world:v7")
                }
            }
        }
    }
}
