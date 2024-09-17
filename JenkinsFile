pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/saitejac614/RestAPP.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'  // Adjust for your build tool
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                // Add deployment steps (e.g., SCP or SSH to deploy to an EC2 instance)
            }
        }
    }
}
