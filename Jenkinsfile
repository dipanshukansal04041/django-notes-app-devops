pipeline {
    agent {
        label 'django-agent'
    }

    stages {
        stage('Agent Test') {
            steps {
                echo 'Pipeline is running on django-agent'
            }
        }

        stage('System Check') {
            steps {
                sh 'whoami'
                sh 'hostname'
                sh 'pwd'
                sh 'git --version'
                sh 'docker --version'
                sh 'kubectl version --client'
            }
        }
    }
}
