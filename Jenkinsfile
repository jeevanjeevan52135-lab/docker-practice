
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jeevan1304/jenkins-docker-demo'
        IMAGE_TAG = '1'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting Source Code'
                checkout scm
            }
        }

        stage('Docker Version') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t "$IMAGE_NAME:$IMAGE_TAG" .'
            }
        }

        stage('Docker Login and Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'Dockerhub auth',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKER_PASS" | docker login \
                            --username "$DOCKER_USER" \
                            --password-stdin

                        docker push "$IMAGE_NAME:$IMAGE_TAG"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image successfully published!'
        }

        failure {
            echo 'Pipeline Failed!'
        }

        always {
            sh 'docker logout || true'
        }
    }
}
