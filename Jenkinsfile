pipeline {
    agent { label 'pk-jnk-agent-2' }  // Specifies the agent where the pipeline kicks off
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
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t kogolop/docker-wpress-db-app1:latest .'
            } 
        }
        // If you have specific test scripts or procedures for your WordPress/MySQL setup,
        // you can include a testing stage here using Docker or Docker Compose commands.
        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "9573e038-44e4-4210-84db-be092e0af109", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push kogolop/docker-wpress-db-app1:latest"
                }
            }
        }
        stage('Deploy with Docker Compose') {
            steps {
                echo "Deploying with Docker Compose..."
                sh "docker-compose down && docker-compose up -d"
            }
        }
        stage('Cleanup Workspace') {
            steps {
                cleanWs() // Cleans the workspace after the build and deployment are done
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
