pipeline {
    agent any

     tools{
         maven "maven3.9"
     }

    environment {
        IMAGE_NAME = "welcome-jenkins:1.0"
        CONTAINER_NAME = "welcome-jenkins-app"
        HOST_PORT = "8081"
        CONTAINER_PORT = "8080"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/phyo-thet-khaing/Hello-World.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build --no-cache -t ${IMAGE_NAME} .
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d \
                      -p ${HOST_PORT}:${CONTAINER_PORT} \
                      --name ${CONTAINER_NAME} \
                      ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deployment Successful!'
        }
        failure {
            echo '❌ Build or Deployment Failed!'
        }
    }
}