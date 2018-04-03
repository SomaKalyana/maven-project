pipeline {
	agent any
	stages{
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
		stage('Deploy to Stage'){
			steps{
				build job: 'deploy-to-staging'

			}
		}
		stage('Deploy to Prod'){
			steps{
				timeout(time:5, unit:'DAYS'){
					input message 'Approve PRODUCTION deployment?'	
				}

				build job: 'deploy-to-prod'
			}
			post {
				success {
					echo 'Code deployed to Production'
				}
				failure {
					echo 'Deploymnet failed'
				}
			}
		}
	}
}