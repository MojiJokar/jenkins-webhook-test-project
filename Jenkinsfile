// A simple Jenkins pipeline example: Done successfully

// pipeline {
//     environment {
//         DOCKER_ID    = "maxjokar2020"      // Your Docker Hub ID
//         DOCKER_IMAGE = "datascientestapi-new" // Your Docker image name
//         DOCKER_TAG   = "v.${BUILD_ID}.0"
//     }

//     agent any

//     stages {

//         stage('Check Environment') {
//             steps {
//                 echo "Jenkins pipeline is running."
//                 sh 'whoami'
//                 sh 'env'
//             }
//         }

//         stage('List Workspace') {
//             steps {
//                 sh 'ls -la'
//             }
//         }

//         stage('Docker Build') {
//             steps {
//                 script {
//                     sh '''
//                         docker rm -f jenkins || true
//                         docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG -f cast-service/Dockerfile cast-service/
//                         sleep 6
//                     '''
//                 }
//             }
//         }

//     }
// }

//2.
// stages {
//   stage('Docker Build') {
//     steps {
//       script {
//         sh '''
//           docker rm -f jenkins || true
//           docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG .
//           sleep 6
//         '''
//       }
//     }
//   }

//   stage('Docker Run') {
//     steps {
//       script {
//         sh '''
//           docker run -d -p 80:80 --name jenkins $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
//           sleep 10
//         '''
//       }
//     }
//   }

//   stage('Test Acceptance') {
//     steps {
//       script {
//         sh 'curl localhost'
//       }
//     }
//   }

//   stage('Docker Push') {
//     environment {
//       DOCKER_PASS = credentials("DOCKER_HUB_PASS")
//     }
//     steps {
//       script {
//         sh '''
//           docker login -u $DOCKER_ID -p $DOCKER_PASS
//           docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
//         '''
//       }
//     }
//   }
// }




//3. another example for docker compse
stages {
  stage('Docker Compose Build') {
    steps {
      script {
        sh '''
          docker-compose build
        '''
      }
    }
  }

//   stage('Docker Compose Up') {
//     steps {
//       script {
//         sh '''
//           docker-compose up -d
//           sleep 10
//         '''
//       }
//     }
//   }

//   stage('Test Acceptance') {
//     steps {
//       script {
//         sh 'curl localhost'
//       }
//     }
//   }

//   stage('Docker Compose Down') {
//     steps {
//       script {
//         sh 'docker-compose down'
//       }
//     }
//   }

//   stage('Docker Compose Push') {
//     environment {
//       DOCKER_PASS = credentials("DOCKER_HUB_PASS")
//     }
//     steps {
//       script {
//         sh '''
//           docker login -u $DOCKER_ID -p $DOCKER_PASS
//           # Push each image defined in your docker-compose.yml individually
//           docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
//           # If you have multiple images, add push commands for each here
//         '''
//       }
//     }
//   }
}

