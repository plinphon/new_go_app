pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("ttl.sh/your-app:2h")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', '') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy on Docker VM') {
            steps {
                sh """
                docker stop myapp || true
                docker rm myapp || true
                docker pull ttl.sh/your-app:2h
                docker run -d -p 4444:4444 --name myapp ttl.sh/your-app:2h
                """
            }
        }
    }
}
