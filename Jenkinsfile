pipeline {
    agent any // This specifies that the pipeline can run on any available agent

    stages {
        stage('Build') { // First stage: Build
            steps {
                echo 'Building...'
                // Add commands here to compile/build your project
                // For example, 'sh './gradlew build'' for a Gradle project
            }
        }
        stage('Test') { // Second stage: Test
            steps {
                echo 'Testing...'
                // Add commands here to test your project
                // For example, 'sh './gradlew test'' for a Gradle project
            }
        }
    }

    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
