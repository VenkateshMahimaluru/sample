def notifywhenfailed() {
echo "hello"
}

node {
        stage('Build') {

               sh "echo This is build"

        }
}
post {
    always {
        notifywhenfailed()
    }
}
