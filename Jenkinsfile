pipeline {
    agent { label "my127ws" }
    environment {
        MY127WS_ENV = "pipeline"
    }
    options {
        compressBuildLog()
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo $GIT_COMMIT'
                milestone(10)
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
