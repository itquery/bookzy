pipeline {
	agent any
	tools {
    	maven 'MVN-3.6.0'
	}
	stages {
    	stage("Checkout") {
        	steps {
        	    git branch: 'master',
                    credentialsId: 'itquery',
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
            	sh "echo Tomcat Deploy!"
			}
       	}

	}
}

