
pipeline {
        agent any
        stages {
        stage('Build') {
                steps {
                        script{
                               
                               echo "${env.BRANCH_NAME}"
                               echo "${env.BUILD_NUMBER}"
                               
                        }
                   
             }
        }
  stage('Check') {
    steps {        
        script {
            Boolean bool = fileExists 'NewFile.txt'
            if (bool) {
                println "The File exists :)"
            } else {
                println "The File does not exist :("
            }   
        }         
    }
}
    }
}

