pipeline {
    agent {
        docker {
            image 'golang:1.22.3'
        }
    }

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials'
        NEXUS_CREDENTIALS_ID = 'nexus-credentials'
        NEXUS_URL = 'http://localhost:8081/repository/test'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/aazh94/homework_devops.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'GOOS=linux go build -a -installsuffix nocgo -o app .'
            }
        }
        stage('Upload to Nexus') {
            steps {
                script {
                    def nexusUrl = "${NEXUS_URL}/app"
                    withCredentials([usernamePassword(credentialsId: "${NEXUS_CREDENTIALS_ID}", passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh '''
                        curl -v -u ${USER}:${PASS} --upload-file ./app ${nexusUrl}
                        '''
                    }
                }
            }
        }
    }
}
