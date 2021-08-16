pipeline {
    agent any
    stages {
        stage('clone') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master'], [name: '*/dev']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/VenkateshMahimaluru/sample.git']]])
            }
        }
        stage('Build') {
            steps {
                echo "This is build"
            }
        }
        stage('Test main') {
            when {
                branch 'main'
            }
            steps {
                echo "This is for testing main branch"
            }
        }
        stage('Test dev') {
            when {
                branch 'dev'
            }
            steps {
                echo "This is for testing dev branch"
            }
        }
    }
}
