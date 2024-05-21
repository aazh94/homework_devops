pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials'
        NEXUS_CREDENTIALS_ID = 'nexus-credentials'
        NEXUS_URL = 'http://localhost:8081/repository/test'
    }

    stages {
        stage('Install Go') {
            steps {
                sh '''
                curl -LO https://golang.org/dl/go1.22.3.linux-amd64.tar.gz
                sudo tar -C /usr/local -xzf go1.22.3.linux-amd64.tar.gz
                export PATH=$PATH:/usr/local/go/bin
                '''
            }
        }
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/aazh94/homework_devops.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh '''
                export PATH=$PATH:/usr/local/go/bin
                GOOS=linux go build -a -installsuffix nocgo -o app .
                '''
            }
        }
        stage('Upload to Nexus') {
            steps {
                script {
                    def nexusUrl = "${NEXUS_URL}/app"
                    withCredentials([usernamePassword(credentialsId: "${NEXUS_CREDENTIALS_ID}", passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh '''
                        export PATH=$PATH:/usr/local/go/bin
                        curl -v -u ${USER}:${PASS} --upload-file ./app ${nexusUrl}
                        '''
                    }
                }
            }
        }
    }
}
