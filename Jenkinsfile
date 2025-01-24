

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
    DOCKERHUB_CREDENTIALS = credentials('DOCKER-CREDENTIAL')
    VERSION = "${env.BUILD_ID}"
    // SONARQUBE_TOKEN = 'squ_32789bcdadb6e4337e432d6cbc100c2a1a14fde5'
    // SONARQUBE_URL = 'http://35.180.137.8:9000/'
    // SONARQUBE_PROJECT_KEY = 'com.ashish.eureka-service'
    // SONARQUBE_LOGIN = 'squ_32789bcdadb6e4337e432d6cbc100c2a1a14fde5'
  }

  tools {
    maven "Maven"
  }

  stages {

    // Step 1: Checkout Latest Code
    stage('Checkout Code') {
      steps {
        // Checkout the latest code from GitHub repository
        git url: 'https://github.com/ashishkumarsingh296/eureka-service.git', credentialsId: 'GITHUB-CREDS'
      }
    }

    // Step 2: Maven Build
    stage('Maven Build') {
        steps {
            script {
//                     bat 'mvn clean install'
//                 }
          // sh 'mvn clean package -DskipTests'
    }
  }

    // Step 3: Run Unit Tests
    // stage('Run Tests') {
    //   steps {
    //     sh 'mvn test'
    //   }
    // }

    // Step 4: SonarQube Analysis
    // stage('SonarQube Analysis') {
    //   steps {
    //     sh """
    //       mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install sonar:sonar \
    //         -Dsonar.host.url=${SONARQUBE_URL} \
    //         -Dsonar.login=${SONARQUBE_LOGIN}
    //     """
    //   }
    // }

    // Step 5: Check Code Coverage in SonarQube
    // stage('Check Code Coverage') {
    //   steps {
    //     script {
    //       def coverageThreshold = 80.0

    //       def response = sh(
    //         script: "curl -H 'Authorization: Bearer ${SONARQUBE_TOKEN}' '${SONARQUBE_URL}/api/measures/component?component=${SONARQUBE_PROJECT_KEY}&metricKeys=coverage'",
    //         returnStdout: true
    //       ).trim()

    //       def coverage = sh(
    //         script: "echo '${response}' | jq -r '.component.measures[0].value'",
    //         returnStdout: true
    //       ).trim().toDouble()

    //       echo "Coverage: ${coverage}"

    //       if (coverage < coverageThreshold) {
    //         error "Coverage is below the threshold of ${coverageThreshold}%. Aborting the pipeline."
    //       }
    //     }
    //   }
    // }

    // Step 6: Docker Build and Push
    stage('Docker Build and Push') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
        sh 'docker build -t ashishdevops1989/eureka-service:${VERSION} .'
        sh 'docker push ashishdevops1989/eureka-service:${VERSION}'
      }
    }

    // Step 7: Cleanup Workspace
    stage('Cleanup Workspace') {
      steps {
        deleteDir()
      }
    }

  }

}
