pipeline {
    agent any

    environment {
        IMAGE_NAME = 'vishaln8890/java-springboot-app'
        DOCKER_CREDENTIALS_ID = 'vishaln8890@gmail.com'
        GIT_CREDENTIALS_ID = 'vishaln8890@gmail.com'
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/vishaln8890/java-springboot-app.git',
                        credentialsId: "${GIT_CREDENTIALS_ID}"
                    ]]
                ])
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Image pushed to Docker Hub!'
        }
    }
}
