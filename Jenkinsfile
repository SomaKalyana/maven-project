pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: '52.41.187.4', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: '54.213.253.39', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages {
		stage('Build') {
			steps{
				bat 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}

		stage ('Deployment') {
			parallel {
				stage('Deploy to Staging') {
					steps {
						sh "scp -i C:/projects/maven-project/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
					}
				}
				stage('Deploy to Production') {
					steps {
						sh "scp -i C:/projects/maven-project/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
					}
				}
					
			}
		}
	}
}