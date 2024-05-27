pipeline {
	agent {label "master"}
	
	environment {
	    APP_NAME = "sos-clientes-ms"
	}
  
	stages {
	    stage('Cleanup Workspace') {
      		steps {
      		    // Checkout the code from the repository
      		    echo "Cleanup Workspace"
      		    script {
                    cleanWs()
                }
      		}
    	}
    	stage('Code Checkout SCM') {
      		steps {
      		    // Checkout the code from the repository
        		echo "Checkout SCM"
                git credentialsId: 'jenkins-loans-statements',
                url: 'https://github.com/KingHrothgar01/sos-clientes-ms-config.git',
                branch: 'master'
      		}
    	}
        stage('Updating Kubernetes Deployment File') {
      		steps {
      		    // Checkout the code from the repository
        		sh "cat deployment-sos-clientes-ms.yaml"
                sh "sed -i 's/${APP_NAME}.*/${APP_NAME}:${VERSION}/g' deployment-sos-clientes-ms.yaml"
                sh "cat deployment-sos-clientes-ms.yaml"
      		}
    	}
    	stage('Push the Changed Deployment File to GIT') {
    		steps {
    		    script {
		       		sh """
                    git add deployment-sos-clientes-ms.yaml
                    git commit -am 'Updated the deployment file' """
                    withCredentials([usernamePassword(credentialsId: 'jenkins-loans-statements',
                                                      usernameVariable: 'GITHUB_APP',
                                                      passwordVariable: 'GITHUB_ACCESS_TOKEN')]) {
                        sh "git push http://$GITHUB_APP:$GITHUB_ACCESS_TOKEN@github.com/KingHrothgar01/sos-clientes-ms-config.git develop"
                    }
		   		}
	        }
    	}
  	}
}