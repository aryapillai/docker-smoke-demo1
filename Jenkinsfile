pipeline {

    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/aryapillai/docker-smoke-demo1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t smoke-demo .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f smoke-test || true

                    docker run -d \
                        --name smoke-test \
                        -p 8084:80 \
                        smoke-demo
                '''
            }
        }

        stage('Smoke Test') {
            steps {
                sh '''
                    sleep 5

                    curl -f http://localhost:8080
                '''
            }
        }

    }

    post {
        always {
            sh '''
                docker rm -f smoke-test || true
            '''
        }
    }
}
