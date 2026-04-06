pipeline {
    agent any

    environment {
        DOCKER_CREDS = credentials('dockerhub-creds')
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Pavithrabca1/pavi-devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t devops-pavi .'
            }
        }

        stage('Tag') {
            steps {
                sh 'docker tag devops-app pavited/devops-pavi:latest'
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKER_CREDS_PSW | docker login -u $DOCKER_USER --password-stdin'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push pavited/devops-pavi:latest'
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
