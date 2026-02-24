pipeline {
  agent any

  environment {
    IMAGE_NAME = "myapp-image"
    CONTAINER_NAME = "myapp-container"
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Riya-rock/pipline-1.git'
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn -v'
        sh 'mvn clean package'
        sh 'ls -lah target'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker version'
        sh 'docker build -t $IMAGE_NAME .'
        sh 'docker images | head -n 5'
      }
    }

    stage('Run Container') {
      steps {
        sh '''
          docker rm -f $CONTAINER_NAME || true
          docker run -d --name $CONTAINER_NAME $IMAGE_NAME
          docker logs $CONTAINER_NAME --tail 50
        '''
      }
    }
  }

  post {
    success { echo "✅ Maven + Docker pipeline success" }
    failure { echo "❌ Pipeline failed" }
  }
}
