pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://localhost:8081/repository/test/'
        NEXUS_USER = 'admin'
        NEXUS_PASSWORD = '12345'
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                    curl -LO https://golang.org/dl/go1.22.3.linux-amd64.tar.gz
                    tar -C /usr/local -xzf go1.22.3.linux-amd64.tar.gz
                    export PATH=$PATH:/usr/local/go/bin
                    GOOS=linux /usr/local/go/bin/go build -a -installsuffix nocgo -o app .
                '''
            }
        }

        stage('Upload to Nexus') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh """
                            curl -v -u ${USER}:${PASS} --upload-file ./app ${NEXUS_URL}
                        """
                    }
                }
            }
        }
    }
}
