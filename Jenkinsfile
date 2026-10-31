pipeline{
    agent any
    environment {
        DOCKER_IMAGE = 'jeeva1304/jenkins-docker-demo'
}
stages {
  stage('checkout') {
    steps {
      echo 'Getting source code'
      checkout scm
    }
  }
  stage('Docker version'){
    steps {
      echo 'Checking Docker version'
      sh 'docker --version'
    }
  }
    stage('Build Docker Image') {
        steps {
        echo 'Building Docker image'
        sh 'docker build -t $DOCKER_IMAGE .'
        }
    }
    stage('Docker Login')
    steps{
        withCredentails([
            usernamepassword(
                credentialsId: 'Dockerhub auth',
                usernameVariable: DOCKER_USER',
                passwordVariable: 'DOCKER_PASS',
            )
        ])
        sh '''
        set +x
        echo "$DOCKER_PASS" | docker login \
        --username "DOCKER_USER" \
        --password-stdin
        '''
    }

}
    }
    stages('push the image to the Docker'){
        steps{
          sh '''
            docker push "$IMAGE_NAME:$1"
            '''
        }
    }
}
stage('push the image to the Docker'){
    steps{
      sh '''
        docker push "$IMAGE_NAME:$1"
        '''
    }
}
    }
    post{
      success{
        echo 'Docker image pushed successfully'
      }
        failure{
          echo 'Failed to push Docker image'
        }
        always{
          sh 'docker logout' || true
            }
        }
    }   