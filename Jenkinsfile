
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fastdeploy:v1'
        KUBECONFIG = '/var/lib/jenkins/kubeconfig'
        HOME = '/home/ubuntu'
        MINIKUBE_HOME = '/home/ubuntu/.minikube'
    }

    options {
        timestamps()
    }

    stages {
        stage('Test') {
            steps {
                echo 'Running application tests...'
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
                echo 'Building Docker image...'
                sh 'docker build -t "$IMAGE_NAME" .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                echo 'Loading Docker image into Minikube...'
                sh '''
                    minikube image load "$IMAGE_NAME" \
                      --profile=minikube
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying application...'
                sh '''
                    kubectl apply -f "$WORKSPACE/k8s/"
                    kubectl rollout restart deployment/fastdeploy
                    kubectl rollout status deployment/fastdeploy --timeout=120s
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                    kubectl get deployments
                    kubectl get pods
                    kubectl get services
                '''
            }
        }
    }

    post {
        success {
            echo 'FastDeploy CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the stage logs above.'
        }

        always {
            echo "Build result: ${currentBuild.currentResult}"
            echo "Build number: ${env.BUILD_NUMBER}"
        }
    }
}

