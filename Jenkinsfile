pipeline {
    agent any

    environment {

        GIT_USERNAME = 'ashishkumarsingh296@gmail.com'
        GIT_PASSWORD = 'Indra@198955'  // OR 'your-password' for HTTPS
       // DOCKER_CREDENTIALS = credentials('DOCKER_CREDENS') // Replace with your credentials ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Manually checkout the code from the Git repository
                git 'https://your-repository-url.git'  // Replace with your Git repository URL
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    // Windows: Run Maven using bat (batch)
                    bat 'mvn clean install'
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
