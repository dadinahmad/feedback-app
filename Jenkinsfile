pipeline {
    agent any

    parameters {
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Centang untuk melakukan deployment setelah build')
    }

    environment {
        IMAGE_NAME = 'feedback-app:latest'
        CONTAINER_NAME = 'feedback-app-container'
        APP_PORT = '8080'
    }

    stages {
        stage('Build') {
            steps {
                echo '📦 Building Docker image feedback-app:latest...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Conditional Deploy') {
            when {
                expression { return params.DEPLOY }
            }
            steps {
                echo '🚀 Melakukan deployment karena parameter DEPLOY = true'
                sh '''
                    if [ $(docker ps -aq -f name=$CONTAINER_NAME) ]; then
                        docker stop $CONTAINER_NAME || true
                        docker rm $CONTAINER_NAME || true
                    fi

                    docker run -d --name $CONTAINER_NAME -p $APP_PORT:80 $IMAGE_NAME
                '''
            }
        }

        stage('Skip Deploy') {
            when {
                expression { return !params.DEPLOY }
            }
            steps {
                echo '⏩ DEPLOY tidak dicentang, melewati tahap deployment.'
            }
        }
    }
}
