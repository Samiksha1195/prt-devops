pipeline {
    agent {
        label 'build-agent'
    }



    environment {
        IMAGE_NAME = "samiksha1195/prt-cicd-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/Samiksha1195/prt-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f prt-container || true
                docker run -d --name prt-container -p 8085:80 $IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }
}
