pipeline {
	/*A Declaritive Pipeline*/

	agent any

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
	}
}