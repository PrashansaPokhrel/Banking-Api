pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'banking-api:latest'
        DOCKER_REGISTRY = 'my-docker-repo/banking-api'
    }

    stages {
        // Build Stage: Compile Code & Create Artifacts
        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    bat 'mvn clean package' // Generates JAR file
                }
            }
        }
        
        // Test Stage: Run Automated Tests
        stage('Test') {
            steps {
                script {
                    echo 'Running unit and integration tests...'
                    bat 'mvn test' // Java JUnit tests
                }
            }
        }

        // Code Quality Stage: Analyze Code Using SonarQube
     stage('Code Quality Analysis') {
    steps {
        script {
            echo 'Running SonarQube code quality check...'
            bat 'sonar-scanner -Dsonar.projectKey=banking-api -Dsonar.sources=src -Dsonar.exclusions=**/test/**'
        }
    }
}

        // Security Scan Stage: Scan Code & Dependencies for Vulnerabilities
        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    bat 'trivy image %DOCKER_IMAGE%' // Scans the Docker image
                }
            }
        }

        // Build Docker Image: Package Microservices
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker Image...'
                    bat 'docker build -t %DOCKER_IMAGE% .'
                }
            }
        }

        // Push Docker Image to Registry
        stage('Push to Registry') {
            steps {
                script {
                    echo 'Pushing Docker image to registry...'
                    bat 'docker tag %DOCKER_IMAGE% %DOCKER_REGISTRY%'
                    bat 'docker push %DOCKER_REGISTRY%'
                }
            }
        }

        // Deploy Application to Local or Cloud Environment
        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    bat 'docker-compose up -d' // Starts services
                }
            }
        }

        // Release to Production (Kubernetes Deployment)
        stage('Release to Production') {
            steps {
                script {
                    echo 'Releasing to production...'
                    bat 'kubectl apply -f deployment.yaml'
                }
            }
        }

        // Monitoring Stage: Set Up Performance & Logging Tools
        stage('Monitoring') {
            steps {
                script {
                    echo 'Setting up monitoring tools...'
                    bat 'prometheus & grafana running' // Start monitoring stack
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
