
pipeline {
    agent any

    environment {
        IMAGE_NAME = "fastdeploy:${BUILD_NUMBER}"
        KUBECONFIG = '/var/lib/jenkins/kubeconfig'
        HOME = '/var/lib/jenkins'
        DOCKER_CONFIG = '/var/lib/jenkins/.docker'
        MINIKUBE_HOME = '/home/ubuntu/.minikube'
    }

    options {
        timestamps()
    }

    stages {
        stage('Test') {
            steps {
                echo 'Running application tests'
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
                echo "Building ${IMAGE_NAME}"
                sh 'docker build -t "$IMAGE_NAME" .'
            }
        }

        stage('Load Image into Minikube') {
            steps {
                echo "Loading ${IMAGE_NAME} into Minikube"
                sh '''
                    minikube image load "$IMAGE_NAME" \
                      --profile=minikube
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying ${IMAGE_NAME}"
                sh '''
                    kubectl apply -f "$WORKSPACE/k8s/"
                    kubectl set image deployment/fastdeploy \
                      fastdeploy="$IMAGE_NAME"
                    kubectl rollout status deployment/fastdeploy \
                      --timeout=120s
                '''
            }
        }

        stage('Verify Deployment') {
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
            echo 'FastDeploy pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the stage logs.'
        }
        always {
            echo "Build result: ${currentBuild.currentResult}"
            echo "Build number: ${env.BUILD_NUMBER}"
        }
    }
}

