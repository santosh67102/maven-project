pipeline {
	agent any
	stages {
		stage ('Build'){
			step {
				sh 'mvn clean package'

			}
		post {
			success {
			echo ' Now Archiving..'
			archiveAtrifacts artifacts:'**/target/*.war'
				}
			
			}
		}
	}
}