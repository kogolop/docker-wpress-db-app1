pipeline {
    agent { label 'pk-jnk-agent-1' }

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

        stage("SonarQube Analysis") {
            steps {
                script {
                    withSonarQubeEnv('pk-sonarqube1') { // Make sure this ID matches your SonarQube server configuration in Jenkins
                        // Use SonarScanner to perform the analysis
                        sh '''
                           sonar-scanner \
                           -Dsonar.projectKey=my-application \  // You can omit this line if SonarQube is configured to auto-generate the project key
                           -Dsonar.sources=. \
                           -Dsonar.host.url=http://192.0.1.244:9000 \
                           -Dsonar.login=$SONARQUBE_TOKEN
                        '''
                    }
                }
            }
        }

        // Your additional stages for Docker operations and cleanup...
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
                withCredentials([usernamePassword(credentialsId: '9573e038-44e4-4210-84db-be092e0af109', passwordVariable: 'DOCKERHUB_PASS', usernameVariable: 'DOCKERHUB_USER')]) {
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
