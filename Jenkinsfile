pipeline {
    agent any

    tools {
        jdk 'jdk' // This must match name in Jenkins > Global Tool Configuration
        maven 'Maven-3.9.10'
    }

    environment {
        IMAGE_NAME = 'java-app'
        CONTAINER_NAME = 'java-app-container'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code already checked out by Jenkins SCM config'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    bat "docker build -t %IMAGE_NAME% ."
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    // Stop and remove existing container if already running
               //     bat "docker stop %CONTAINER_NAME% || echo Not running"
                 //   bat "docker rm %CONTAINER_NAME% || echo Already removed"
                    bat "docker run -d --name %CONTAINER_NAME% -p 8080:8080 %IMAGE_NAME%"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build and deployment succeeded!'
        }
        failure {
            echo 'Build or deployment failed.'
        }
    }
}
