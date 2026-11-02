pipeline {
  agent any
  tools { nodejs 'node' }
  environment {
    PORT      = "${env.BRANCH_NAME == 'main' ? '3000' : '3001'}"
    IMAGE     = "node${env.BRANCH_NAME}:v1.0"
    CONTAINER = "node${env.BRANCH_NAME}"
  }
  stages {
    stage('Build') {
      steps {
        sh 'node -v'
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'CI=true npm test'
      }
    }
    stage('Docker build') {
      steps {
        sh 'docker build -t $IMAGE .'
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          docker rm -f $CONTAINER || true
          docker run -d --name $CONTAINER -p $PORT:3000 $IMAGE
        '''
      }
    }
  }
}
