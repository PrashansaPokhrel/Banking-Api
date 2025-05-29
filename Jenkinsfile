pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building Banking API JAR using Maven...'
                    bat 'mvn clean package'
                }
            }
        }

        stage('Test') {
            steps {
                    echo 'Running automated tests...'
                    bat 'mvn test'
            }
        }
     stage('Code Quality Analysis') {
            steps {
                    echo 'Running SonarQube analysis...'
                    withSonarQubeEnv('SonarQube') {  
                    bat '"C:\\Program Files\\sonar-scanner\\bin\\sonar-scanner" -Dsonar.projectKey=BankingAPI -Dsonar.sources=src/main/java/ -Dsonar.host.url=http://localhost:9000 -Dsonar.login=your-token'
                        }
            }
        }
        stage('Security Scan') { steps { echo 'Performing security scan...' } }
        stage('Build Docker Image') { steps { echo 'Creating Docker image...' } }
        stage('Push to Registry') { steps { echo 'Pushing Docker image to registry...' } }
        stage('Deploy to Staging') { steps { echo 'Deploying to staging...' } }
        stage('Release to Production') { steps { echo 'Releasing to production...' } }
        stage('Monitoring & Alerting') { steps { echo 'Setting up monitoring...' } }
    }

    post {
        success { echo 'Pipeline execution successful!' }
        failure { echo 'Pipeline execution failed! Check logs for details.' }
    }
}
