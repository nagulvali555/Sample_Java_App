pipeline {

	agent any

	stages {

		stage('Static Code Analysis') {
			steps {
				withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonarqube' ) {
				  sh 'chmod +x gradlew'
					sh './gradlew sonarqube'
				}
			}
		}

		// No need to occupy a node
		stage("Quality Gate"){
	    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
		    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
		    if (qg.status != 'OK') {
		      error "Pipeline aborted due to quality gate failure: ${qg.status}"
		    }
	    }
	  }

	}
}