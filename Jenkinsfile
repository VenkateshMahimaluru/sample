
pipeline {
        agent any
        stages {
        stage('Build') {
                steps{
                        sh '''
                                if [[ ${BRANCH_NAME} == 'test' ]]; then 
                                        echo "this is test"
                                        "mkdir newfolder"
                                }
                                else {
                                        echo "This is build" }
                            '''
              
             }
        }
    }
}

