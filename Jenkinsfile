
@Library('my-shared-library') _

pipeline {
    agent { label 'django-agent' }

    environment {
        IMAGE_NAME = 'dipanshuk16/django-notes-app'
        IMAGE_TAG = 'latest'
        K8S_NAMESPACE = 'django-ns'
        DEPLOYMENT_NAME = 'django-deployment'
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                dockerBuild("${IMAGE_NAME}:${IMAGE_TAG}")
            }
        }

        stage('Docker Push') {
            steps {
                dockerPush(
                    "${IMAGE_NAME}:${IMAGE_TAG}",
                    "${DOCKER_CREDENTIALS}"
                )
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                k8sDeploy("${K8S_NAMESPACE}", '.')
            }
        }

        stage('Kubernetes Verify') {
            steps {
                verifyK8s("${DEPLOYMENT_NAME}", "${K8S_NAMESPACE}")
            }
        }
    }
}

