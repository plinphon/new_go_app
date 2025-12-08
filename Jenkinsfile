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
                sshagent(['24df0688-3e5d-48b8-9678-1d0d14635fe3']) {
                    sh '''
                        # Copy binary to target VM
                        scp -o StrictHostKeyChecking=no myapp laborant@172.16.0.3:/home/laborant/myapp
                        
                        # Create systemd service file remotely
                        ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo bash -c '
cat > /etc/systemd/system/myapp.service <<EOF
[Unit]
Description=My Go App
After=network.target

[Service]
User=laborant
ExecStart=/home/laborant/myapp
Restart=always

[Install]
WantedBy=multi-user.target
EOF
'"

                        # Reload systemd and start the service
                        ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo systemctl daemon-reload && sudo systemctl enable myapp && sudo systemctl restart myapp"
                    '''
                }
            }
        }
    }
}
