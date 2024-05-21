pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials' 
        NEXUS_CREDENTIALS_ID = 'nexus-credentials' 
        NEXUS_URL = 'http://localhost:8081/repository/test/'  
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
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
                    withCredentials([usernamePassword(credentialsId: NEXUS_CREDENTIALS_ID, passwordVariable: 'NEXUS_PASSWORD', usernameVariable: 'NEXUS_USERNAME')]) {
                        sh '''
                            curl -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file ./app ${NEXUS_URL}app
                        '''
                    }
                }
            }
        }
    }
}
