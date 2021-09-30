node {
        stage('Build') {
            steps {
                echo "This is build"
            }
        }
}
post {
    always {
        notifywhenfailed()
    }
}
