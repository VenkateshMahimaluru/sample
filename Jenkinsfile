pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				emailext body: 'input message: Do you need this?', subject: 'Test', to: 'venkatesh.mahimaluru@accenture.com'
					}
			}
		}
	}
}
