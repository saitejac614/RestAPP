pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKERHUB_REPO = 'saitejac614/restapp'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub
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
                    // Push the image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        def image = docker.image("${DOCKERHUB_REPO}:${env.BUILD_ID}")
                        image.push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up the Docker images
            sh 'docker rmi ${DOCKERHUB_REPO}:${env.BUILD_ID} || true'
        }
        success {
            echo 'Build, Test, and Docker Push completed successfully.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
