pipeline {
	agent any
	
	parameters {
		string (name: 'tomcat_stage', defaultValue: '10.0.2.12', description: 'Staging Tomcat Instance')
		string (name: 'tomcat_prod', defaultValue: '10.0.2.8', description: 'Production Tomcat Instance')
	}
	
	triggers {
		pollSCM ('* * * * *')
	}
	
	stages {
		stage ('Build') {
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
		
		stage ('Deployments') {
			parallel {
				stage ('Deploy to Staging') {
					steps {
						sh "scp -i /home/jenkins/BAHDevKeyPair.pem **/target/*.war ubuntu@${params.tomcat_stage}:/opt/tomcat/latest/webapps"
					}
				}
				
				stage ('Deploy to Production') {
					steps {
						sh "scp -i /home/jenkins/BAHDevKeyPair.pem **/target/*.war ubuntu@${params.tomcat_prod}:/opt/tomcat/latest/webapps"
					}
				}
			}
		}
	}
}
