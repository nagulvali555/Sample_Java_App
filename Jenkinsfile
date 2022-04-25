pipeline {

	agent any

	options {
		buildDiscarder(logRotator(numToKeepStr: '1'))
	}

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
			steps {

				script {
			    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
            def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
            if (qg.status != 'OK') {
              error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
          }
				}

			}
	  }

	  stage("Build") {
	    steps {
				sh './gradlew clean build'
	    }
	  }

    stage("Test") {
      steps {
        sh './gradlew test'
      }
    }

    stage("Package docker image") {
	    steps {
	      sh 'creating docker image...'
	    }
    }

    stage("Pushing docker image") {
      steps {
        sh 'pushing docker image to repo'
      }
    }

    stage("Deploying to staging") {
      steps {
        sh 'Deploying to staging...'
      }
    }
	}

	post {
    always {
      mail bcc: '', body: '${env.JOB_NAME} <br> Build Number: ${env.BUILD_NUMBER} <br> URL de build : ${env.BUILD_URL}', cc: '', from: '', replyTo: '', subject: '', to: 'nagulvali555@gmail.com'
    }
  }
}