pipeline {
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
    }
}

