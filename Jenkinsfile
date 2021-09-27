pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				script {
					 env.BUILD_EMAIL_RECIPIENT='venkatesh.mahimaluru@accenture.com'
				}
				emailext (
				      subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
				      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
					<p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
				      to: "$BUILD_EMAIL_RECIPIENT"
    					)
				
			}
		}
	}
}
