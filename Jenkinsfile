
pipeline {
        agent any
        stages {
        stage('Build') {
                steps {
                        script{
                               if ("${env.BRANCH_NAME}" == "test") {
                                println "this is test"
                                bat mkdir new
                            } else {
                                println "The File does not exist :("
                            }  
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

