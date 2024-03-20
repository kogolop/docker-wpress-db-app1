pipeline {
    agent {
	label 'pk-jnk-agent1'
	}                         // This specifies that the pipeline can run on any available agent
    stages {
       stage('clone'){
            steps{echo 'Cloning Stage'
            git url:'https://github.com/kogolop/docker-wpress-db-app1.git' , branch:"main"}
	    echo 'Pulled Code Successfully From Github'	
            
        }   
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
