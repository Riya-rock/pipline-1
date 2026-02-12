pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/username/repo.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the application'
               bat 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests'
                mvn test
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application'
            }
        }
    }
}
