pipeline {
	agent any

	stages {
		stage('Build') {
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
		stage('Deploy to Staging') {
			steps {
				build job: 'Deploy-to-staging'
			}
		}
		stage('Deploy to Production') {
			steps {
				timeout(time:5, unit:'Days') {
					input message: 'Approve PRODUCTION Deployment?'
				}
				
				build job: 'Deploy-to-prod'
			}
			post {
				success {
					echo 'Code deployed to production.'
				}
				
				failure {
					echo 'Deployment failed.'
				}
			}
		}
	}
}