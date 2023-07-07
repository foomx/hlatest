pipeline {
    agent {
        kubernetes {
            inheritFrom 'nodejs-builder'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                // Checkout source code from version control system
                checkout scm
                
                // Install Node.js dependencies
                sh 'npm install -dd'
                
                // Build your Node.js project
                sh 'npm run build'
            }
        }
        
        stage('Test') {
            steps {
                // Run tests (e.g., using Jest)
                sh 'npm test'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube scanner
                withSonarQubeEnv('sonarqube') {
                    sh 'sonar-scanner'
                }
            }
        }
        
        stage('Publish Docker Image') {
            steps {
                // Login to Nexus Docker repository
                script {
                    docker.withRegistry('https://10.151.38.166:8081/repository/docker-group/', 'jenkinsnexus') {
                        // Build and tag Docker image
                        sh 'docker build -t my-app .'
                        
                        // Push Docker image to Nexus repository
                        sh 'docker push my-app'
                    }
                }
            }
        }
    }
}
