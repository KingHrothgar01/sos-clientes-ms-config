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
                git url: 'https://github.com/KingHrothgar01/sos-clientes-ms-config.git',
                branch: 'master'
      		}
    	}
        stage('Updating Kubernetes Deployment File') {
      		steps {																																			
      		    // Checkout the code from the repository
        		sh "cat deployment-sos-clientes-ms.yaml"
                sh "sed -i 's#kinghrothgar01/${APP_NAME}.*#kinghrothgar01/${APP_NAME}:${VERSION}#g' deployment-sos-clientes-ms.yaml"
                sh "cat deployment-sos-clientes-ms.yaml"
      		}
    	}																																					
    	stage('Push the Changed Deployment File to GIT') {
    		steps {
				script {
					withCredentials([gitUsernamePassword(credentialsId: 'sos-loans-github', gitToolName: 'localGit')]) {
                    	sh "git config user.name 'Jenkins Pipeline'"
						sh "git config user.email 'jenkins@localhost'"
						sh "git add deployment-sos-clientes-ms.yaml"
						sh "git commit -am 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
						sh "git push -u origin master"
					}
				}
			}
    	}
  	}
}