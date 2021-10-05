pipeline {
    agent any
	stages {
		stage("Wait for Remote System") {

			  // Call a remote system to start execution, passing a callback url
			  sh "curl -X POST -H 'Content-Type: application/json' -d '{\"callback\":\"${env.BUILD_URL}input/app/abort\"}' http://httpbin.org/post"

			  // Block and wait for the remote system to callback
			  input id: 'app', message: 'Do you want to approve?'
			}

		stage('pre-build') {
			steps  {
				script {
					 env.BUILD_EMAIL_RECIPIENT='venkatesh.mahimaluru@accenture.com'
				}
				
				emailext body: """<p>STARTED: Job \'${env.JOB_NAME} [${env.BUILD_NUMBER}]\':</p>
				      		<a href="DescribeChangeSet.txt" target="_blank">Textual</a>
						<p>Check console output at &QUOT;<a href=\'${env.BUILD_URL}\'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""", 
						subject: 'Hello, Test email', 
						to: 'venkatesh.mahimaluru@accenture.com'
				
			}
		}
	}
}
