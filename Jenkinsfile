pipeline {

	agent any

	stages {

		stage('Quality Check') {
			steps {
				withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonarqube' ) {
					sh '/usr/local/share/sonar-scanner/bin/sonar-scanner'
				}
			}
		}
	}
}