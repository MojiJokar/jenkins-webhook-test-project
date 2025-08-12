// A simple Jenkins pipeline example
pipeline {
    environment {
        DOCKER_ID    = "maxjokar2020"         // Your Docker Hub ID
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
                        docker build -t $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG \
                            -f cast-service/Dockerfile cast-service/
                        sleep 6
                    '''
                }
            }
        }

        // Optional login stage — uncomment if you need Docker Hub push before running
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

        stage('Debug Kubeconfig') {
            steps {
                sh '''
                    echo "📌 API Server address in kubeconfig:"
                    grep server /var/lib/jenkins/.kube/config

                    echo ""
                    echo "📌 First line of Certificate Authority data:"
                    grep certificate-authority-data /var/lib/jenkins/.kube/config | head -1

                    echo ""
                    echo "📌 Entire kubeconfig content (for verification):"
                    cat /var/lib/jenkins/.kube/config
                '''
            }
        }

        stage('Deploy to Dev') {
            environment {
                KUBECONFIG = credentials('config')  // Jenkins credential holding kubeconfig
            }
            steps {
                script {
                    sh '''
                        mkdir -p ~/.kube
                        cat $KUBECONFIG > ~/.kube/config

                        # Verify cluster connectivity
                        kubectl get nodes

                        # Your deployment commands here, e.g.:
                        kubectl create namespace dev --dry-run=client -o yaml | kubectl apply -f -
                        helm upgrade --install your-app fastapi-project/ --namespace dev
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    sh '''
                        docker rm -f jenkins || true
                        docker rmi $DOCKER_ID/$DOCKER_IMAGE:$DOCKER_TAG || true
                    '''
                }
            }
        }

    }
}
