pipeline {
    agent any

    environment {
        K8S_SERVER = 'https://kubernetes:6443'
        K8S_CREDENTIALS_ID = 'k8s-token'
        POD_NAME = 'myapp'
        IMAGE = 'ttl.sh/myapp:1h'
    }

    stages {
        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: env.K8S_CREDENTIALS_ID, serverUrl: env.K8S_SERVER]) {
                    sh '''#!/bin/bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: '${POD_NAME}'
spec:
  containers:
  - name: '${POD_NAME}'
    image: '${IMAGE}'
EOF
'''
                }
            }
        }
    }
}
