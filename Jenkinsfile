pipeline {
    agent none
    environment {
        MY127WS_ENV = "pipeline"
    }
    stages {
        stage('Build') {
            agent { label "my127ws" }
            steps {
                sh 'echo $GIT_COMMIT'
                sh 'env'
                milestone(10)
            }
            post {
                always {
                    cleanWs()
                }
            }
        }
        stage('Deploy') {
            agent { label "my127ws" }
            when {
                not { triggeredBy 'TimerTrigger' }
                anyOf {
                    branch 'main'
                    branch 'develop'
                    branch pattern: "release/*"
                }
            }
            steps {
                sh 'echo $GIT_COMMIT'
                sh 'env'
            }
            post {
                always {
                    cleanWs()
                }
            }
        }
    }
}
