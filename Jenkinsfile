pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps  {
				
				sh '''#!/bin/bash
				rm -f prop.properties
				echo -ne 'CIUUID='>>prop.properties
				uuidgen>>prop.properties

				echo -ne 'RANDOM_STRING='>>prop.properties
				random_string=$(head /dev/urandom | tr -dc A-Za-z0-9 | head -c 13 ; echo '')
				echo $random_string>>prop.properties
				'''
			script {
				def props = readProperties file: 'prop.properties'
				env.CIUUID = props.CIUUID
				env.RANDOM_STRING = props.RANDOM_STRING
				export TARGET_ACCOUNT_ID= `cat deployment.yaml  | yq .$BRANCH_NAME.accountId`
				echo $TARGET_ACCOUNT_ID
				export AWS_DEFAULT_REGION=`cat deployment.yaml | yq -r .region`
				echo $AWS_DEFAULT_REGION
				export ROLE="arn:aws:iam::$TARGET_ACCOUNT_ID:role/LambdaExecutionRole"
				echo "========  assuming permissions => $ROLE ========="
				account_role=`aws sts assume-role --role-arn $ROLE --role-session-name "jenkins-prismacode-$CIUUID"`
				export AWS_ACCESS_KEY_ID=$(echo $account_role | jq -r .Credentials.AccessKeyId)
				echo $AWS_ACCESS_KEY_ID
				export AWS_SECRET_ACCESS_KEY=$(echo $account_role | jq -r .Credentials.SecretAccessKey)
				echo $AWS_SECRET_ACCESS_KEY
				export AWS_SESSION_TOKEN=$(echo $account_role | jq -r .Credentials.SessionToken)
				echo $AWS_SESSION_TOKEN
				export AWS_SECURITY_TOKEN=$(echo $account_role | jq -r .Credentials.SessionToken)
				echo $AWS_SECURITY_TOKEN
				}

			sh "echo The CI UUID is $CIUUID"
			sh "echo The Random String is $RANDOM_STRING"
			}
		}
	}
}
