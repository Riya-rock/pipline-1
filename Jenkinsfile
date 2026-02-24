pipeline {
  agent any

  environment {
    IMAGE_NAME = "riya-nginx-app"
    CONTAINER_NAME = "riya-nginx-container"
    HOST_PORT = "8081"   // Jenkins is 8080, so app is 8081
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main',
        url: 'https://github.com/Riya-rock/pipline-1.git'
      }
    }

    stage('Maven Validate') {
      steps {
        sh 'mvn -v'
        sh 'mvn -q validate'
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker version'
        sh 'docker build -t $IMAGE_NAME:latest .'
        sh 'docker images | head -n 5'
      }
    }

    stage('Run') {
      steps {
        sh '''
          docker rm -f $CONTAINER_NAME 2>/dev/null || true
          docker run -d -p $HOST_PORT:80 --name $CONTAINER_NAME $IMAGE_NAME:latest
          docker ps | grep $CONTAINER_NAME || true
        '''
      }
    }
  }

  post {
    success { echo "✅ SUCCESS: Open http://192.168.112.128:8081" }
    failure { echo "❌ FAILED" }
  }
}
