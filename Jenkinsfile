pipeline {
  agent any

  environment {
    IMAGE_NAME = "riya-demo-app"
    CONTAINER_NAME = "riya-demo-container"
    HOST_PORT = "8080"
    CONTAINER_PORT = "8080"
  }

  stages {

    stage('Checkout Code') {
      steps {
        git branch: 'main',
        url: 'https://github.com/Riya-rock/pipline-1.git'
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn -v'
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
        sh 'docker images | head -n 5'
      }
    }
stage('Run Docker Container') {
  steps {
    sh '''
      docker stop $CONTAINER_NAME 2>/dev/null || true
      docker rm   $CONTAINER_NAME 2>/dev/null || true

      docker run -d \
        -p 8081:$CONTAINER_PORT \
        --name $CONTAINER_NAME \
        $IMAGE_NAME:latest

      docker ps | grep $CONTAINER_NAME || true
    '''
      }
    }

    stage('Run Docker Container') {
      steps {
        sh '''
          docker run -d \
          -p $HOST_PORT:$CONTAINER_PORT \
          --name $CONTAINER_NAME \
          $IMAGE_NAME:latest

          docker ps | grep $CONTAINER_NAME || true
        '''
      }
    }
  }

  post {
    success {
      echo '✅ Riya Pipeline Success — Docker image built & container running!'
    }
    failure {
      echo '❌ Build failed!'
    }
  }
}
