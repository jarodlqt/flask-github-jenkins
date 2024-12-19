pipeline {
    agent any
    environment {
        IMAGE_NAME = "flask-github-jenkins"
    }
    stages {
        stage('Clone Repository') {
            steps {
            git branch: 'master', url: 'https://github.com/jarodlqt/flask-github-jenkins.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest tests/
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        docker.image("${IMAGE_NAME}:latest").push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé.'
        }
    }
}
