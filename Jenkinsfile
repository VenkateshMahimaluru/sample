pipeline {
    agent any
	stages {
		stage('pre-build') {
			steps {
				timeout(activity: true, time: 60, unit: 'MINUTES') {
				input message "Do you want to proceed for Production deployment?", ok: 'Yes'
				}
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
}
