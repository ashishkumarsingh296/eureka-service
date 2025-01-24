

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

    environment {
        DOCKER_IMAGE_NAME = 'ashishdevops1989/eureka-service'
        DOCKER_TAG = "0.0.${BUILD_NUMBER}"  // Use Jenkins build number for versioning
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
                    // Compile, test, and generate coverage report
                    bat 'mvn clean install'
                }
            }
        }

        // stage('Run Unit Tests and Code Coverage') {
        //     steps {
        //         script {
        //             // Run Maven to execute tests and code coverage (if using JaCoCo or another tool)
        //             bat 'mvn test'
        //             bat 'mvn jacoco:report'  // Assuming JaCoCo for code coverage
        //         }
        //     }
        // }

        stage('Login to Docker') {
            steps {
                script {
                    // Login to Docker using Jenkins credentials
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
                        echo "Successfully logged in to Docker Hub"
                    }
                }
            }
        }

        stage('Stop and Remove Existing Containers') {
            steps {
                script {
                    // Stop and remove any existing containers with the same name
                    sh 'docker ps -q -f "name=eureka-service" | xargs -r docker stop'
                    sh 'docker ps -a -q -f "name=eureka-service" | xargs -r docker rm'
                }
            }
        }

        stage('Remove Existing Docker Image') {
            steps {
                script {
                    // Remove the existing Docker image if it exists
                    sh "docker images -q ${DOCKER_IMAGE_NAME}:${DOCKER_TAG} | xargs -r docker rmi"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with the latest tag
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the newly built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").push()
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container, mapping port 8761
                    // This will expose port 8761 on the host machine
                    sh "docker run -d -p 8761:8761 --name eureka-service ${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                }
            }
        }
    }
}
