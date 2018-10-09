pipeline {
	/*A Declaritive Pipeline*/

	agent any

	tools {
		maven 'localMaven'
		jdk 'localJDK'
	}

	stages {
		stage ('Build and Archive') {

			steps {
				sh 'mvn clean package'
			}
			
			post {
				success {
					echo 'Now archiving...'
					archiveArtifacts artifacts: '**/*.war'
				}
			}

		}
	
		stage ('Deploy to Staging') {
			
			steps{
				build job: 'deploy-to-stage'
			}
			
		}
	}
}