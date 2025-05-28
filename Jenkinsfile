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
                    echo 'Running SonarQube analysis...'
                    bat 'sonar-scanner -Dsonar.projectKey=banking-api -Dsonar.sources=src'
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

        // Deploy to Test Environment (Staging Server or Docker Compose)
        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging environment...'
                    bat 'docker-compose up -d' // Starts services
                }
            }
        }

        // Promote Application to Production (AWS CodeDeploy)
        stage('Release to Production') {
            steps {
                script {
                    echo 'Releasing to production...'
                    bat 'aws deploy create-deployment --application-name BankingAPI --deployment-group-name ProdGroup --s3-location bucket=my-artifacts,key=banking-api.zip,bundleType=zip'
                }
            }
        }

        // Monitoring & Alerting in Production
        stage('Monitoring & Alerting') {
            steps {
                script {
                    echo 'Setting up production monitoring...'
                    bat 'datadog-agent start' // Starts Datadog agent for monitoring
                    bat 'newrelic start' // Starts New Relic for performance metrics
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline execution successful!'
        }
        failure {
            echo 'Pipeline execution failed! Check logs for details.'
        }
    }
}
