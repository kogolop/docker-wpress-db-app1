pipeline {
    agent {
        label 'pk-jnk-agent-2'  // This specifies the particular agent where there pipe kicks off from
	  tools {
       		 jdk 'Java17'
       		 maven 'Maven1' // applying maven as my test tool
	    }  
    }
    stages {
	stage('Cleanup Workspace'){
	    steps {
		 cleanWs()         //this function prepare the workspace for check out from SCM github
	     }
	 }
        stage('Clone') {
            steps {
                echo 'Cloning Stage..'
                // Ensure there's no spacing in the URL
	        git credentialsId: 'cc60d203-7c05-46b1-9aac-5b3715663691', url: 'https://github.com/kogolop/docker-wpress-db-app1.git', branch: "main"
                echo 'Pulled Code Successfully From Github'
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                // Ensure there's no leading space before docker command
                sh 'docker build -t kogolop/docker-wpress-db-app1:latest .'
             } 
	}
	stage("Test Application"){
           steps {
		 echo 'Testing...'  
                 sh "mvn test"
           }
        }
        stage("push-to-docker") {
            steps {
                echo "Pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId: "9573e038-44e4-4210-84db-be092e0af109", 
		passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")])
		{
                    sh "docker tag docker-wpress-db-app1 kogolop/docker-wpress-db-app1:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push kogolop/docker-wpress-db-app1:latest"
                }
            }
        }
        stage("deploy") {
    	   steps {
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
            echo 'Build Succeeded!'
	    echo 'Push  Succeeded!'
	    echo 'Deploy Succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
