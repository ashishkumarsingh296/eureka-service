pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('DOCKER_CREDENS') // Replace with the actual credentials ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/ashishkumarsingh296/eureka-service.git', credentialsId: 'GITHUB-CREDS'
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    bat 'mvn clean install' // Or './mvnw clean install' for Linux
                }
            }
        }

        stage('Login to Docker') {
            steps {
                script {
                    // Log in to Docker registry using the injected credentials
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        echo "Successfully logged in to Docker Hub"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build('ashishdevops1989/eureka-service:0.0.1')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        docker.image('ashishdevops1989/eureka-service:0.0.1').push()
                    }
                }
            }
        }
    }
}
