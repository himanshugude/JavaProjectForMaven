pipeline {
    agent any

    tools {
        jdk 'jdk' // This must match the name in Jenkins > Global Tool Configuration
        maven 'Maven-3.9.10'
    }

    environment {
        IMAGE_NAME = 'java-app'
        CONTAINER_NAME = 'java-app-container'
        SONARQUBE_SERVER = 'SonarQubeServer' // Name configured in Jenkins > Manage Jenkins > Configure System
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${env.SONARQUBE_SERVER}") {
                    withCredentials([string(credentialsId: 'SonarQube-Token', variable: 'SONAR_TOKEN')]) {
                        bat 'mvn sonar:sonar -Dsonar.login=%SONAR_TOKEN%'
                    }
                }
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
                    bat "docker stop %CONTAINER_NAME% || echo Not running"
                    bat "docker rm %CONTAINER_NAME% || echo Already removed"
                    bat "docker run -dt --name %CONTAINER_NAME% -p 8082:8082 %IMAGE_NAME%"
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
