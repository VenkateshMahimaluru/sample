pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				emailext subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
				      	body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p> \n
						<p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
					to: venkatesh.mahimaluru@accenture.com 
			}
		}
	}
}
