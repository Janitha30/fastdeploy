
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fastdeploy'
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
                      -v "$WORK
