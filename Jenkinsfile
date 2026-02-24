pipeline {
  agent any

  environment {
    IMAGE_NAME = "myapp-image"
    CONTAINER_NAME = "myapp-container"
    HOST_PORT = "8080"
    CONTAINER_PORT = "8080"
  }

  stages {

    stage('Checkout Code') {
      steps {
        git branch: 'main',
        url: 'https://github.com/your-repo/your-project.git'
      }
    }

    stage('Maven Build') {
      steps {
        sh 'mvn clean package -DskipTests'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME:latest .'
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
        '''
      }
    }
  }

  post {
    success { echo '✅ Application deployed successfully!' }
    failure { echo '❌ Build failed!' }
  }
}
