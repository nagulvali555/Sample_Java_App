pipeline {

	agent any

	stages {

		stage('Quality Check') {
			steps {
				withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonarqube' ) {
					sh './gradle sonarqube'
				}
			}
		}
	}
}