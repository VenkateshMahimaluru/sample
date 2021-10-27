
pipeline {
        agent any
        stages {
        stage('Build') {
                steps{
                if( env.BRANCH_NAME == 'test')
                        echo "this is test"
                else
                      echo "This is build"
        }
        }
    }
}

