pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'banking-api:latest'
        DOCKER_REGISTRY = 'my-docker-repo/banking-api'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    bat 'mvn clean package'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    bat 'mvn test'
                }
            }
        }

        stage('Code Quality') {
            steps {
                script {
                    echo 'Analyzing code quality...'
                    bat 'sonar-scanner'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    bat 'trivy image %DOCKER_IMAGE%'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker Image...'
                    bat 'docker build -t %DOCKER_IMAGE% .'
                }
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    echo 'Pushing Docker image to registry...'
                    bat 'docker tag %DOCKER_IMAGE% %DOCKER_REGISTRY%'
                    bat 'docker push %DOCKER_REGISTRY%'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    bat 'docker-compose up -d'
                }
            }
        }

        stage('Release to Production') {
            steps {
                script {
                    echo 'Releasing to production...'
                    bat 'kubectl apply -f deployment.yaml'
                }
            }
        }

        stage('Monitoring') {
            steps {
                script {
                    echo 'Setting up monitoring tools...'
                    bat 'prometheus & grafana running'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline execution successful!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}
