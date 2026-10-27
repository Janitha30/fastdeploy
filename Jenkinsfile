
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fastdeploy:v1'
        KUBECONFIG = '/var/lib/jenkins/kubeconfig'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Janitha30/fastdeploy.git'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker run --rm \
                      -v "$WORKSPACE:/app" \
                      -w /app \
                      python:3.11-slim \
                      sh -c "pip install --quiet -r requirements.txt && pytest -q"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t fastdeploy:v1 .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                echo 'Image loading will be handled separately by Ubuntu'
             }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                    kubectl apply -f "$WORKSPACE/k8s/"
                    kubectl rollout status deployment/fastdeploy --timeout=120s
                '''
            }
        }
    }

    post {
        always {
            sh 'date'
            echo "Pipeline duration: ${currentBuild.durationString}"
        }
    }
}
