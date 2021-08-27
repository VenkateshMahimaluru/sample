pipeline {
    agent {
        docker {
            image 'sample:latest'
            label 'docker'
            registryUrl 'https://389627665088.dkr.ecr.us-east-1.amazonaws.com'
            args '-u 0:0'
        }
    }
		stage('pre-build') {
			input {
				timeout(activity: true, time: 60) {
				message "Do you want to proceed for Production deployment?", ok: 'Yes'
			}
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
}
    
