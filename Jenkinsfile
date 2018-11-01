pipeline {
	/*A Declaritive Pipeline*/

	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'IP address of dev tomcat server')
		string(name: 'tomcat_prod', defaultValue: 'localhost', description: 'IIP address of prod tomcat server')
	}
	
	triggers {
		pollSCM ('* * * * *')
	}

	tools {
		maven 'localMaven'
		jdk 'localJDK'
	}

	stages {
		
		stage ('Build and Test') {
			parallel {
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
	
				stage ('Checkstyle') {
			
					steps {
						sh 'mvn checkstyle:checkstyle'
					}
			
				}
			}
		}
		
		stage ('Deploy to Staging') {
			
			steps{
				sh "scp -o StrictHostKeyChecking=no **/target/*.war root@${params.tomcat_dev}:/opt/apache-tomcat-8.5.34-staging/webapps/"
			}
			
		}
		
		stage ('Deploy to Prod'){
		
			steps {
				timeout(time: 5, unit: 'DAYS') {
					input message: 'Do you approve PRODUCTION deployment?'
				}
				
				sh "scp -o StrictHostKeyChecking=no **/target/*.war root@${params.tomcat_prod}:/opt/apache-tomcat-8.5.34-prod/webapps/"
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