pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'go version'
                sh 'go build -o myapp main.go'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Deploy step placeholder"
                '''
            }
        }
    }
}
