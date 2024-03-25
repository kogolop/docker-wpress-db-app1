pipeline {
    agent { label 'pk-jnk-agent-1' }  // Specifies the Jenkins agent label

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
                    withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                        sh """
                           sonarqube-scanner \
                           -Dsonar.projectKey=myProjectKey \
                           -Dsonar.sources=. \
                           -Dsonar.host.url=http://192.0.1.244:9000 \
                   //        -Dsonar.login=$SONAR_TOKEN
                            """
                    }
                }
                echo 'SonarQube Analysis Completed'
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
                withCredentials([usernamePassword(credentialsId: "9573e038-44e4-4210-84db-be092e0af109", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh """
                       docker login -u $dockerhubuser -p $dockerhubpass
                       docker push kogolop/docker-wpress-db-app1:latest
                       """
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
                cleanWs() // Cleans the workspace after the build and deployment are done
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
