pipeline {
    agent any

    environment {
        EC2_USER = 'ec2-user'
        EC2_HOST = '98.89.34.13'
        DOCKER_IMAGE = 'plinphonpat/myapp:latest'
        SSH_CREDENTIALS = 'ec2-ssh-key'
    }

    stages {
        stage('Deploy to EC2') {
            steps {
                sshagent (credentials: [env.SSH_CREDENTIALS]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} \\
                        'docker pull ${DOCKER_IMAGE} && \\
                         docker stop myapp || true && \\
                         docker rm myapp || true && \\
                         docker run -d -p 4444:4444 --name myapp ${DOCKER_IMAGE}'
                    """
                }
            }
        }
    }
}
