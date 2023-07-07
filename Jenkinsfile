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
                //sh 'npm run start'
                sh 'npm install'
                
                // Build your Node.js project
                //sh 'npm run build'
            }
        }
        
        
        stage('SonarQube Analysis') {
            steps {
                // Execute SonarQube scanner
                script{
                    def scannerHome = tool 'sonarqubescanner';
                    withSonarQubeEnv('jenkinsonarkey') {
                        sh "${scannerHome}/sonar-scanner -Dsonar.java.binaries=src/main/java -Dsonar.projectKey=test"
                    }
                }
            }
        }
        
        stage('Publish Docker Image') {
            steps {
                // Login to Nexus Docker repository
                script {
                    docker.withRegistry('https://10.151.38.166:8081/repository/docker-group/', 'jenkinsnexus') {
                        // Build and tag Docker image
                       // sh 'docker build -t my-app .'
                        app = docker.build("test:latest")
                        app.push("test:latest")
                        // Push Docker image to Nexus repository
                        //sh 'docker push my-app'
                    }
                }
            }
        }
    }
}
