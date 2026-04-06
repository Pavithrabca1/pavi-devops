pipeline {
    agent any

    environment {
        DOCKER_USER = credentials('dockerhub-creds').username
        DOCKER_PASS = credentials('dockerhub-creds').password
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Pavithrabca1/pavi-devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t devops-pavi .'
            }
        }

        stage('Tag') {
            steps {
                sh 'docker tag devops-app $DOCKER_USER/devops-pavi:latest'
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $DOCKER_USER/devops-pavi:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f devops-container || true
                docker run -d -p 3002:5000 --name devops-container $DOCKER_USER/devops-pavi:latest
                '''
            }
        }
    }
}
