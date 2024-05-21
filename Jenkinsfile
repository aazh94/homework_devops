pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/aazh94/homework_devops.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                script {
                    sh '/usr/local/go/bin/go test .'
                }
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build('ubuntu-bionic:8082/hello-world:v7')
                }
            }
        }
        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('http://ubuntu-bionic:8082', 'docker-credentials') {
                        docker.image('ubuntu-bionic:8082/hello-world:v7').push()
                    }
                }
            }
        }
    }
}
