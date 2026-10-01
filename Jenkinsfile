pipeline {

    agent {
        label 'django-agent'
    }

    environment {
        IMAGE_NAME = 'dipanshuk16/django-notes-app'
        IMAGE_TAG = 'latest'
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh '''
                    echo "===== SYSTEM ====="
                    whoami
                    hostname
                    pwd

                    echo "===== GIT ====="
                    git --version

                    echo "===== DOCKER ====="
                    docker --version

                    echo "===== KUBERNETES ====="
                    kubectl version --client
                '''
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh '''
                    docker build \
                        -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Push') {
            steps {
                echo 'Logging into Docker Hub and pushing image...'

                withCredentials([
                    usernamePassword(
                        credentialsId: "${DOCKER_CREDENTIALS}",
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                            --username "$DOCKER_USER" \
                            --password-stdin

                        docker push ${IMAGE_NAME}:${IMAGE_TAG}

                        docker logout
                    '''
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                echo 'Deploying application to Kubernetes...'

                sh '''
                    kubectl apply -f namespace.yml
                    kubectl apply -f deployment.yml
                    kubectl apply -f service.yml
                '''
            }
        }

        stage('Kubernetes Verify') {
            steps {
                echo 'Checking Kubernetes resources...'

                sh '''
                    echo "===== PODS ====="
                    kubectl get pods -n django-ns

                    echo "===== DEPLOYMENT ====="
                    kubectl get deployment -n django-ns

                    echo "===== SERVICE ====="
                    kubectl get service -n django-ns
                '''
            }
        }
    }

    post {

        success {
            echo '======================================'
            echo 'CI/CD PIPELINE COMPLETED SUCCESSFULLY'
            echo '======================================'
        }

        failure {
            echo '======================================'
            echo 'CI/CD PIPELINE FAILED'
            echo 'Check the failed stage above.'
            echo '======================================'
        }
    }
}
