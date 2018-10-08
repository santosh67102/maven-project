pipeline {
    agent any
	tools{
		maven 'localMaven'
		jdk 'localJDK'
	}

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'ec2-18-221-241-151', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-18-221-39-42', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/santoshgautam/Downloads/tomcat.pem /Users/santoshgautam/maven-project/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/santoshgautam/Downloads/tomcat.pem /Users/santoshgautam/maven-project/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
// pipeline {
// 	agent any
// 	tools{
// 		maven 'localMaven'
// 		jdk 'localJDK'
// 	}
// 	stages {
// 		stage ('Build'){
// 			steps {
// 				sh 'mvn clean package'

// 			}
// 		post {
// 			success {
// 			echo 'Now Archiving..'
// 			archiveArtifacts artifacts:'**/target/*.war'
// 				}
			
// 			}
// 		}
// 		stage ('Deploy to Staging'){
//             steps {
//                 build job: 'Deploy-to-staging'
//             }
//         }
// 		stage ('Deploy to Production'){
//             steps{
//                 timeout(time:5, unit:'DAYS'){
//                     input message:'Approve PRODUCTION Deployment?'
//                 }

//                 build job: 'Deploy-to-prod'
//             }
//             post {
//                 success {
//                     echo 'Code deployed to Production.'
//                 }

//                 failure {
//                     echo ' Deployment failed.'
//                 }
//             }
//         }


// 		}
// 	}