pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds')
        IMAGE_NAME = "rakesh612/docker-nginx-app"
        CONTAINER_NAME = "docker_job"
    }

    stages {

        stage("git clone") {
            steps {
                git branch: 'main', url:'https://github.com/chauhan46/docker-nginx-app.git' #if u want to clone u required, this is my executed script
            }
        }

        stage('Build docker image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage("Login to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push image') {
            steps {
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }

        stage('Run container') {
    steps {
        sh '''
          docker stop $CONTAINER_NAME || true
          docker rm $CONTAINER_NAME || true

          docker run -d \
            --name $CONTAINER_NAME \
            -p 80:80 \
            ${IMAGE_NAME}:latest
        '''
    }
}

    }
}
