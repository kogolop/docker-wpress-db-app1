pipeline {
    agent { label 'pk-jnk-agent-1' }

    stages {
        // Your previous stages...
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube Analysis...'
                withSonarQubeEnv('pk-sonarqube1') {
                    // Ensure your script block uses triple single quotes for multi-line scripts in Groovy
                    sh '''
                       sonar-scanner \
                       -Dsonar.projectKey=my-application \  // Replace 'my-application' with your actual project key
                       -Dsonar.sources=. \
                       -Dsonar.host.url=http://192.0.1.244:9000 \
                       -Dsonar.login=${env.SONARQUBE_TOKEN}
                    '''
                }
                echo 'SonarQube Analysis Completed'
            }
        }
        
        // Your subsequent stages...
    }
    post {
        // Your post-build actions...
    }
}
