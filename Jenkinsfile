pipeline {

	agent any

	stages {

		stage('Quality Check') {
			steps {
				withSonarQubeEnv(credentialsId: 'sonar-token') {
					sh './gradle sonarqube'
				}
			}
		}
	}
}