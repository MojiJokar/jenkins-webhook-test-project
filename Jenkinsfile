// A simple Jenkins pipeline example
pipeline {
    environment {
        DOCKER_ID    = "maxjokar2020"          // Your Docker Hub ID
        DOCKER_IMAGE = "datascientestapi-new"  // Your Docker image name
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
                        docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG \
                            -f cast-service/Dockerfile cast-service/
                        sleep 6
                    '''
                }
            }
        }

        // Optional login stage — uncomment if you need Docker Hub push
        /*
        stage('Docker Login') {
            steps {
                script {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_ID --password-stdin
                    '''
                }
            }
        }
        */

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

        stage('Test Acceptance') { 
            steps { 
                script { 
                    sh 'curl localhost' 
                } 
            } 
        } 

        stage('Docker Push') { 
            environment { 
                DOCKER_PASS = credentials("DOCKER_HUB_PASS") 
            } 
            steps { 
                script { 
                    sh '''
                        docker login -u $DOCKER_ID -p $DOCKER_PASS
                        docker push $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG
                    ''' 
                } 
            } 
        } 
        
        stage('Deploy to Dev') { 
            environment { 
                KUBECONFIG = credentials("config") 
            } 
            steps { 
                script { 
                    sh ''' 
                            rm -rf ~/.kube && mkdir ~/.kube
                            # Write the entire kubeconfig file from Jenkins credential
                            cat $KUBECONFIG > ~/.kube/config

                            # Optionally extract CA cert for explicit TLS usage (if needed)
                            kubectl config view --raw -o jsonpath='{.clusters[0].cluster.certificate-authority-data}' | base64 --decode > ~/.kube/ca.crt

                            cp charts/values.yaml values.yml
                            sed -i "s+tag.*+tag: ${DOCKER_TAG}+g" values.yml

                            kubectl create namespace dev --dry-run=client -o yaml | kubectl apply -f -
                            helm upgrade --install app charts --values=values.yml --namespace dev
            
                    ''' 
                } 
            } 
        }
    }
}
