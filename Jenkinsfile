pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				script {
					 env.BUILD_EMAIL_RECIPIENT='venkatesh.mahimaluru@accenture.com'
				}
				input id 'app' message 'Do you want to approve?'
				emailext body: """<p>STARTED: Job \'${env.JOB_NAME} [${env.BUILD_NUMBER}]\':</p>
				      		<a href="DescribeChangeSet.txt" target="_blank">Textual</a>
						<p>Check console output at &QUOT;<a href=\'${env.BUILD_URL}\'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""", 
						subject: 'Hello, Test email', 
						to: 'venkatesh.mahimaluru@accenture.com'
				
			}
		}
	}
}
