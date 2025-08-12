// A simple Jenkins pipeline example
// A simple Jenkins pipeline example
pipeline {
    environment {
        DOCKER_ID    = "maxjokar2020"      // Your Docker Hub ID
        DOCKER_IMAGE = "datascientestapi-new" // Your Docker image name
        DOCKER_TAG   = "v.${BUILD_ID}.0"
    }

    agent any

    stages {

        stage('Check Environment') {
            steps {
                echo "Jenkins pipeline is running."
                sh 'whoami'
                sh 'env'
            }
        }

        stage('List Workspace') {
            steps {
                sh 'ls -la'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh '''
                        docker rm -f jenkins || true
                        docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG -f cast-service/Dockerfile cast-service/
                        sleep 6
                    '''
                }
            }
        }
        // stage('Docker Login') {
        //     steps {
        //         script {
        //             sh '''
        //                 echo $DOCKER_PASSWORD | docker login -u $DOCKER_ID --password-stdin
        //             '''
        //         }
        //     }
        // }
         
        stage('Docker Run') { 
            steps { 
                script { 
                sh ''' 
                docker run -d -p 80:80 --name jenkins \
                        $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG 
                sleep 10 
                ''' 
                } 
            } 
        }

    }
}


//2. another example of a Jenkins pipeline

