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
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}

		}
	
		stage ('Deploy to Staging') {
			
			steps{
				build job: 'deploy-to-stage'
			}
			
		}
		
		stage ('Deploy to Prod'){
		
			steps {
				timeout(time: 5, unit: 'DAYS') {
					input message: 'Do you approve PRODUCTION deployment?'
				}
				
				build job: 'deploy-to-prod'
			}
			
			post {
				success {
					echo 'Deployment to Production successful'
				}
				failure {
					echo 'Deployment to Production failed'
				}
			}
			
		}
	}
}