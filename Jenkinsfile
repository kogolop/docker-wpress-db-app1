pipeline {
    agent {
        label 'pk-jnk-agent-2'  // This specifies that the pipeline can run on any available agent
    }
    stages {
        stage('Clone') {
            steps {
                echo 'Cloning Stage'
                // Ensure there's no spacing in the URL
                git url: 'https://github.com/kogolop/docker-wpress-db-app1.git', branch: "main"
                echo 'Pulled Code Successfully From Github'
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
                // Ensure there's no leading space before docker command
                sh 'docker build -t kogolop/docker-wpress-db-app1:latest .'
            }
        }
        stage("push-to-docker") {
            steps {
                echo "Pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId: "9573e038-44e4-4210-84db-be092e0af109", passwordVariable:    		"dockerhubpass", usernameVariable: "dockerhubuser")])
		{
                    sh "docker tag docker-wpress-db-app1 kogolop/docker-wpress-db-app1:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push kogolop/docker-wpress-db-app1:latest"
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // Commands to test your project would go here
            }
        }
	stage("Deploy") {
    	   steps {
		   //jenkins deploying using dcoker compose file: stopping and removing old containers to replace with new created containers
        	echo "Deploying the container"
               	sh "docker-compose down && docker-compose up -d"    
    	    }
	}

    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'Build succeeded!'
	    echo 'Push  succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
