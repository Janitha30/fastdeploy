pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fastdeploy:v1'
        KUBECONFIG = '/var/lib/jenkins/kubeconfig'
    }

    stages {
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
                echo 'Image loading handled separately for this test'
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
            echo "Build result: ${currentBuild.currentResult}"
            echo "Pipeline duration: ${currentBuild.durationString}"
        }
    }
}

