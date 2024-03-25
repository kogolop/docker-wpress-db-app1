pipeline {
    agent { label 'pk-jnk-agent-1' }  // This should match your Jenkins agent label

    environment {
        // Define environment variables for Docker Hub credentials
        DOCKERHUB_CREDENTIALS = credentials('9573e038-44e4-4210-84db-be092e0af109')
        // SonarQube token is stored as a Jenkins credential
        SONARQUBE_TOKEN = credentials('jenkins-sonarqube-token')
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning Repository...'
                git credentialsId: 'cc60d203-7c05-46b1-9aac-5b3715663691', 
                    url: 'https://github.com/kogolop/docker-wpress-db-app1.git', 
                    branch: "main"
                echo 'Repository Cloned Successfully'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Analysis...'
                withSonarQubeEnv('pk-sonarqube1') { // Ensure 'pk-sonarqube1' matches your SonarQube server configuration in Jenkins
                    sh '''
                       sonar-scanner \
                       -Dsonar.sources=. \
                       -Dsonar.host.url=http://192.0.1.244:9000 \
                       -Dsonar.login=${SONARQUBE_TOKEN}
                    '''
                }
                echo 'SonarQube Analysis Completed'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t kogolop/docker-wpress-db-app1:latest .'
                echo 'Docker Image Built Successfully'
            }
        }
        
        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS, passwordVariable: "DOCKERHUB_PASS", usernameVariable: "DOCKERHUB_USER")]) {
                    sh '''
                       docker login -u $DOCKERHUB_USER -p $DOCKERHUB_PASS
                       docker push kogolop/docker-wpress-db-app1:latest
                    '''
                }
                echo 'Docker Image Pushed Successfully'
            }
        }
        
        stage('Deploy with Docker Compose') {
            steps {
                echo "Deploying with Docker Compose..."
                sh 'docker-compose down && docker-compose up -d'
                echo 'Deployment Completed Successfully'
            }
        }
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                echo 'Workspace Cleaned'
            }
        }
    }
    post {
        always {
            echo 'This section will always execute.'
        }
        success {
            echo 'Build, Push, and Deployment Succeeded!'
        }
        failure {
            echo 'The process failed at some point.'
        }
    }
}
