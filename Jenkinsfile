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


//}

// docker compose  :
pipeline {
    agent any

    environment {
        DOCKER_ID = 'maxjokar2020'
        DOCKER_TAG = "v.${BUILD_ID}.0"
        // Make sure DOCKER_PASS is configured in Jenkins Credentials with id 'DOCKER_HUB_PASS'
    }

    stages {

        stage('Docker Compose Build') {
            steps {
                script {
                    echo "Building Docker images via Docker Compose..."
                    sh '''
                        docker-compose build
                    '''
                }
            }
        }

        stage('Docker Compose Up') {
            steps {
                script {
                    echo "Starting containers with Docker Compose..."
                    sh '''
                        docker-compose up -d
                        sleep 10
                    '''
                }
            }
        }

        // stage('Test Acceptance') {
        //     steps {
        //         script {
        //             echo "Running acceptance tests..."
        //             // Customize this to your test command
        //             sh 'curl localhost'
        //         }
        //     }
        // }

        // stage('Docker Compose Down') {
        //     steps {
        //         script {
        //             echo "Stopping and removing all containers..."
        //             sh 'docker-compose down'
        //         }
        //     }
        // }

        // stage('Docker Login and Push Images') {
        //     environment {
        //         DOCKER_PASS = credentials('DOCKER_HUB_PASS')
        //     }
        //     steps {
        //         script {
        //             echo "Logging into Docker Hub..."
        //             sh """
        //                 docker login -u $DOCKER_ID -p $DOCKER_PASS
        //             """

        //             echo "Pushing Docker images defined in docker-compose.yml..."
        //             // You need to push each image manually if multiple images exist in docker-compose.yml
        //             sh """
        //                 docker push $DOCKER_ID/datascientestapi:$DOCKER_TAG
        //                 # Add additional push commands for other images here if applicable
        //             """
        //         }
        //     }
        // }
    }

    // post {
    //     always {
    //         echo 'Cleaning up any running containers...'
    //         sh 'docker-compose down || true'
    //     }
    // }
}
