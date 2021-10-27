
pipeline {
        agent any
        stages {
        stage('Build') {
                steps{
                        script {
                                if( env.BRANCH_NAME == 'test') {
                                        echo "this is test"
                                        sh "mkdir newfolder"
                                }
                else
                      echo "This is build"
                        }
                }
        }
    }
}

