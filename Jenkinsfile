

// working pipeline but password exposing here not secure
// pipeline {
//     agent any

//     environment {
//         DOCKER_USERNAME = 'ashishdevops1989'
//         DOCKER_PASSWORD = 'Indra@1962'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git url: 'https://github.com/ashishkumarsingh296/eureka-service.git', credentialsId: 'GITHUB-CREDS'
//             }
//         }

//         stage('Build with Maven') {
//             steps {
//                 script {
//                     bat 'mvn clean install'
//                 }
//             }
//         }

//         stage('Login to Docker') {
//             steps {
//                 script {
//                     // Secure Docker login using --password-stdin
//                     bat """
//                     echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin
//                     """
//                 }
//             }
//         }

//         stage('Build Docker Image') {
//             steps {
//                 script {
//                     docker.build('ashishdevops1989/eureka-service:0.0.1')
//                 }
//             }
//         }

//         stage('Push Docker Image') {
//             steps {
//                 script {
//                     docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
//                         docker.image('ashishdevops1989/eureka-service:0.0.1').push()
//                     }
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/ashishkumarsingh296/eureka-service.git', credentialsId: 'GITHUB-CREDS'
            }
        }

        stage('Build with Maven') {
            steps {
                script {
                    bat 'mvn clean install'
                }
            }
        }

        stage('Login to Docker') {
            steps {
                script {
                    // Login to Docker using credentials stored in Jenkins securely
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
                        echo "Successfully logged in to Docker Hub"
                    }
                }
            }
        }

         stage('Build Docker Image') {
            steps {
                script {
                    docker.build('ashishdevops1989/eureka-service:0.0.1')
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('ashishdevops1989/eureka-service:0.0.1')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub using the secure credentials
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
                        docker.image('ashishdevops1989/eureka-service:0.0.1').push()
                    }
                }
            }
        }
    }
}


