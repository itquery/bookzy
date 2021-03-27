pipeline {
	agent any
	stages {
    	stage("Checkout") {
        	steps {
        	    git branch: 'master',
                    credentialsId: 'itquery-git',
                    url: 'https://github.com/itquery/bookzy.git'
        	}
    	}
    	stage('Build') {
        	steps {
					sh "mvn -f web-app/pom.xml clean install"
        	}
    	}

    	stage("JUnit test") {
        	steps {
            	sh "mvn -f web-app/pom.xml verify"
			}
            post {
                always {
					junit 'web-app/target/surefire-reports/*.xml'
					}
					}
    	}
    	stage("Deploy") {
        	steps {
			deploy adapters: [tomcat8(credentialsId: 'tomcatDeployer', path: '', url: 'http://localhost:7000')], contextPath: 'bookzy', war: 'web-app/target/*.war'
			}
		post {
		success {  
             		echo 'Deployment is successful'  
				}  
               failure {
			emailext body: '${env.JOB_NAME}: Deployment failed', recipientProviders: [developers()], subject: '${env.JOB_NAME}:Deployment failed', to: 'itquery@gmail.com'
			}
			}
       	}

	}
}

