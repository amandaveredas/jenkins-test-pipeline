pipeline {
    agent any
    tools { nodejs 'node' }
    environment {
        DOCKERHUB_CRED = credentials('dockerhub')
    }
    stages{ 
        stage('Build') {
            steps {
                sh """
                    docker build -f Dockerfile.jenkins -t jenkins-node-sample:latest .
                    docker tag jenkins-node-sample amandaveredas/jenkins-node-sample:latest
                    docker tag jenkins-node-sample amandaveredas/jenkins-node-sample:$BUILD_NUMBER
                """
            }
        }
        stage('Test') {
            steps {
                sh 'npm install'
                sh """
                    npm test
                """
            }
        }
        stage('Publish') {
            steps {
                sh 'echo $DOCKERHUB_CRED_PSW | docker login -u $DOCKERHUB_CRED_USR --password-stdin'
                sh """
                    docker push amandaveredas/jenkins-node-sample:latest
                    docker push amandaveredas/jenkins-node-sample:$BUILD_NUMBER
                """
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}