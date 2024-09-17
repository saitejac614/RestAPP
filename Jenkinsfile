pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Replace with your Jenkins Docker Hub credentials ID
        DOCKERHUB_REPO = 'saitejac614/restapp' // Replace with your Docker Hub repository name
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout code from your GitHub repository
                git branch: 'main', url: 'https://github.com/saitejac614/RestAPP.git'
            }
        }

        stage('Build Java Application') {
            steps {
                // Build the Java application using Maven
                sh 'mvn clean package'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests using Maven
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    def image = docker.build("${DOCKERHUB_REPO}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        def image = docker.image("${DOCKERHUB_REPO}:${env.BUILD_ID}")
                        image.push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            script {
                sh 'docker rmi ${DOCKERHUB_REPO}:${env.BUILD_ID} || true'
            }
        }
        success {
            echo 'Build, Test, and Docker Push completed successfully.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
