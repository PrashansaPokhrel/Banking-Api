pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    echo 'Running unit tests...'
                    sh 'mvn test'
                }
            }
        }

        stage('Code Quality') {
            steps {
                script {
                    echo 'Analyzing code quality...'
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Performing security scan...'
                    sh 'trivy image <your-docker-image>'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo 'Releasing to production...'
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }

        stage('Monitoring') {
            steps {
                script {
                    echo 'Setting up monitoring tools...'
                    sh 'prometheus & grafana running'
                }
            }
        }
    }
}
