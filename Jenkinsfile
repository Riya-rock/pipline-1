pipeline {
  agent any

  environment {
    IMAGE_NAME = "riya-demo-app"
    CONTAINER_NAME = "riya-demo-container"
    HOST_PORT = "8081"
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

    stage('Stop & Remove Old Container') {
      steps {
        sh '''
          docker stop $CONTAINER_NAME 2>/dev/null || true
          docker rm   $CONTAINER_NAME 2>/dev/null || true
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
          docker logs $CONTAINER_NAME --tail 20 || true
        '''
      }
    }
  }

  post {
    success { echo '✅ Docker image built & container running on 8081!' }
    failure { echo '❌ Build failed!' }
  }
}
