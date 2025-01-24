

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
    DOCKER_CREDENTIAL= credentials('DOCKER-CREDENTIAL')
    VERSION = "${env.BUILD_ID}"
    IMAGE_NAME = 'ashishdevops1989/eureka-service'
    CONTAINER_NAME = 'eureka-service-container'
    PORT = '8761'
  }

  tools {
    maven "Maven"
  }

  stages {

    // Step 1: Checkout Latest Code
    stage('Checkout Code') {
      steps {
        git url: 'https://github.com/ashishkumarsingh296/eureka-service.git', credentialsId: 'GITHUB-CREDS'
      }
    }

    // Step 2: Maven Build
    stage('Maven Build') {
      steps {
        script {
          bat 'mvn clean package -DskipTests'
        }
      }
    }

    // Step 3: Run Unit Tests
    stage('Run Tests') {
      steps {
        script {
          bat 'mvn test'
        }
      }
    }

    // // Step 4: Stop Running Docker Container (if exists)
    // stage('Stop Docker Container') {
    //   steps {
    //     script {
    //       // Stop the container if it is already running
    //       bat "docker stop ${CONTAINER_NAME} || true"
    //     }
    //   }
    // }

    // // Step 5: Remove Docker Container (if exists)
    // stage('Remove Docker Container') {
    //   steps {
    //     script {
    //       // Remove the container if it exists
    //       bat "docker rm ${CONTAINER_NAME} || true"
    //     }
    //   }
    // }

    // // Step 6: Remove Docker Image (if exists)
    // stage('Remove Docker Image') {
    //   steps {
    //     script {
    //       // Remove the Docker image if it exists (if not already removed)
    //       bat "docker rmi ${IMAGE_NAME}:${VERSION} || true"
    //     }
    //   }
    // }



     // Step 4: Stop Running Docker Container (if exists)
    stage('Stop Docker Container') {
      steps {
        script {
          // Stop the container if it is already running (Windows)
          bat "docker stop ${CONTAINER_NAME} || exit /b 0"
        }
      }
    }

    // Step 5: Remove Docker Container (if exists)
    stage('Remove Docker Container') {
      steps {
        script {
          // Remove the container if it exists (Windows)
          bat "docker rm ${CONTAINER_NAME} || exit /b 0"
        }
      }
    }

    // Step 6: Remove Docker Image (if exists)
    stage('Remove Docker Image') {
      steps {
        script {
          // Remove the Docker image if it exists (if not already removed)
          bat "docker rmi ${IMAGE_NAME}:${VERSION} || exit /b 0"
        }
      }
    }

    // Step 7: Build Docker Image
    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:${VERSION}")
        }
      }
    }

    // Step 8: Push Docker Image
    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'DOCKER-CREDENTIAL') {
            docker.image("${IMAGE_NAME}:${VERSION}").push()
          }
        }
      }
    }

    // Step 9: Run Docker Container
    stage('Run Docker Container') {
      steps {
        script {
          // Run the container with port mapping (8761)
          sh "docker run -d --name ${CONTAINER_NAME} -p ${PORT}:${PORT} ${IMAGE_NAME}:${VERSION}"
        }
      }
    }

    // Step 10: Cleanup Workspace
    stage('Cleanup Workspace') {
      steps {
        deleteDir()
      }
    }

  }
}

