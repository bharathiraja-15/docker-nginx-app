pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/bharathiraja-15/docker-nginx-app'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t bharathiraja3234/my_app:latest .'
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker_hub_cr',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push bharathiraja3234/my_app:latest'
            }
        }
        stage('Run Container') {
            steps {
                sh '''
                    docker stop my_app || true
                    docker rm my_app || true
                    docker pull bharathiraja3234/my_app:latest
                    docker run -d --name my_app -p 8081:80 bharathiraja3234/my_app:latest
                '''
            }
        }
    }
}
