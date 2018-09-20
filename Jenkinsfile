pipeline {
	agent any
	
	parameters {
		string (name: 'tomcat_stage', defaultValue: '54.87.193.159', description: 'Staging Tomcat Instance')
		string (name: 'tomcat_prod', defaultValue: '35.172.192.77', description: "Production Tomcat Instance')
	}
	
	triggers {
		pollSCM ('* * * * *')
	}
	
	stage ('Deployments) {
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
