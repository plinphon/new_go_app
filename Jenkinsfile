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
                echo "Copying binary to target VM..."
                scp -o StrictHostKeyChecking=no myapp laborant@172.16.0.3:/home/laborant/myapp

                echo "Creating systemd service..."
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "echo '[Unit]
Description=Simple Go App
After=network.target

[Service]
ExecStart=/usr/local/bin/myapp
Restart=always
User=laborant

[Install]
WantedBy=multi-user.target' | sudo tee /etc/systemd/system/myapp.service > /dev/null"

                echo "Moving binary into /usr/local/bin"
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo mv /home/laborant/myapp /usr/local/bin/myapp"
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo chmod +x /usr/local/bin/myapp"

                echo "Reloading systemd and starting service"
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo systemctl daemon-reload"
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo systemctl enable myapp"
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo systemctl restart myapp"

                echo "Checking status..."
                ssh -o StrictHostKeyChecking=no laborant@172.16.0.3 "sudo systemctl status myapp --no-pager"
                '''
            }
        }
    }
}
