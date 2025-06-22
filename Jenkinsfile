pipeline {
    agent any

    environment {
        IMAGE_NAME = 'feedback-app:latest'
        CONTAINER_NAME = 'feedback-app-container'
        APP_PORT = '8080'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/dadinahmad/feedback-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker image feedback-app:latest...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Manual Approval (QA)') {
            steps {
                input message: 'Approve to deploy?', ok: 'Deploy or Abort'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Docker container feedback-app:latest...'
                sh '''
                    docker ps -q --filter name=$CONTAINER_NAME | grep -q . && docker stop $CONTAINER_NAME && docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p $APP_PORT:80 $IMAGE_NAME
                '''
            }
        }
    }
}
