pipeline {
    agent any

    environment {
        IMAGE_NAME = "naufaalg/jenkins-test:latest"  // ganti dengan nama Docker Hub kamu
        DOCKERHUB_CRED = 'dockerhub-creds'          // credential yang kamu buat di Jenkins
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/NaufaalG/jenkins-test.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CRED}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $IMAGE_NAME
                    """
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop test-container || true"
                    sh "docker rm test-container || true"
                    sh "docker run -d --name test-container -p 80:80 $IMAGE_NAME"
                }
            }
        }
    }
}
