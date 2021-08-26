pipeline {
    agent {
        docker {
            image 'sample:latest'
            label 'docker'
            registryUrl 'https://389627665088.dkr.ecr.us-east-1.amazonaws.com'
            args '-u 0:0'
        }
    }
    stages {
		stage('pre-build') {
			steps{
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
			}

			sh "echo The CI UUID is $CIUUID"
			sh "echo The Random String is $RANDOM_STRING"

			}
		}
		stage('validation')   {
			parallel {
				stage('scan-black') {
					steps {
					echo "Running scan stage..."
					sh '''#!/bin/bash
						set +e
						echo "--- Formatting Check for Python source code  ---"
						black -v src/
					'''
					}
				}
			stage('scan-flake8') {
					steps {
					echo "Running scan stage..."
					sh '''#!/bin/bash
						set +e
						echo "--- Lint Check for Python source code  ---"
						flake8 -v src/
					'''
					}
				}
			stage('Security-check') {
				steps {
					echo "Running security check..."
					sh '''#!/bin/bash
						set +e
						echo "--- Security check for Python source code ---"
						bandit -r src/*.py
					'''
					}
				}
			}
		}
		stage('packaging-Generate Dev Package') {
			when {
				branch 'dev'
			}

			steps {
				sh '''#!/bin/bash
					zip -r prisma_code_remediation.zip *
				'''
			}
		}
		stage('packaging-Generate Prod Package') {
			when {
				branch 'prod'
			}

			steps {
				sh '''#!/bin/bash
					zip -r prisma_code_remediation.zip *
				'''
			}
		}
		stage('deployment-deploy-dev') {
			when {
				branch 'dev'
				}
			steps {
				echo "Deploying..."
				sh '''#!/bin/bash
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

					aws lambda update-function-code --function-name  my-function --zip-file fileb://function.zip --publish
				
				'''
				}
			}
		stage(creating_aliases-dev) {
			steps {
				echo "Creating aliases..."
				sh '''#!/bin/bash
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

					aws lambda update-alias --function-name my-function --name Stable --function-version $(aws lambda update-function-code --function-name  my-function --zip-file fileb://function.zip --publish | jq '.Version')
				'''
				}   
			}
		stage('deployment-deploy-prod') {
			when {
				branch 'prod'
			}

			input {
				message "Do you want to proceed for Production deployment?"
			}
			steps {
				echo "Deploying..."
				sh '''#!/bin/bash
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

					aws lambda update-function-code --function-name  my-function --zip-file fileb://function.zip --publish        
				'''
				}
			}
		stage(creating_aliases-prod) {
			steps {
				echo "Creating aliases..."
				sh '''#!/bin/bash
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

					aws lambda update-alias --function-name my-function --name Stable --function-version $(aws lambda update-function-code --function-name  my-function --zip-file fileb://function.zip --publish | jq '.Version')
				'''
			}   
		}
	}
}
    
