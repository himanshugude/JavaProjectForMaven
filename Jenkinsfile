pipeline {
    agent any

    tools {
        jdk 'jdk' // This name must match the JDK name set in Jenkins > Global Tool Configuration
        maven 'Maven-3.9.10' // Same here for Maven
    }

    environment {
        GIT_REPO = 'https://github.com/himanshugude/simple-java-project-1.git'
    }

    stages {
        stage('Checkout') {
            steps {
                // You don't need to specify Git repo here if you're using "Pipeline from SCM"
                echo "Code already checked out by Jenkins SCM config"
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
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
