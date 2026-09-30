pipeline {
    agent {
        label 'django-agent'
    }

    stages {

        stage('Agent Test') {
            steps {
                sh 'whoami'
                sh 'hostname'
            }
        }

        stage('Docker Test') {
            steps {
                sh 'docker --version'
                sh 'docker ps'
            }
        }

        stage('Kubernetes Test') {
            steps {
                sh 'kubectl version --client'
                sh 'kubectl get nodes'
            }
        }
    }
}
