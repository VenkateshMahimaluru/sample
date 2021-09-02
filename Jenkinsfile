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
					bat '''
						export ROLE="arn:aws:iam::389627665088:role/lambda-ex"
						echo "========  assuming permissions => $ROLE ========="
						account_role=`aws sts assume-role --role-arn $ROLE --role-session-name "jenkins-prismacode-$CIUUID"`
					'''
					}
			}
		}
	}
}
