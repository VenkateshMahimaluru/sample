
pipeline {
        agent any
        stages {
        stage('Build') {
                steps{
                        sh '''
                                if [[ env.BRANCH_NAME == 'test' ]]; then 
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

