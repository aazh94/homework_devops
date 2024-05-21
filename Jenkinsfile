pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/aazh94/homework_devops.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'go test .'
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("ubuntu-bionic:8082/hello-world:v7")
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('http://ubuntu-bionic:8082', 'nexus') {
                        docker.image("ubuntu-bionic:8082/hello-world:v7").push()
                    }
                }
            }
        }
    }
}
