pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				withCredentials([[
					    $class: 'AmazonWebServicesCredentialsBinding',
					    credentialsId: "AWSCredsSBX",
					    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
					    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
					]]) {
					   	 bat "echo this is ${env.AWS_ACCESS_KEY_ID}"
               					 bat "echo this is ${env.AWS_SECRET_ACCESS_KEY}"
					}
			}
		}
	}
}
