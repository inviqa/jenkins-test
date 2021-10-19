pipeline {
    agent none
    environment {
        MY127WS_ENV = "pipeline"
    }
    stages {
        stage('Build') {
            agent { label "my127ws-preview" }
            steps {
                sh 'echo $GIT_COMMIT'
                sh 'env | sort -n'
                milestone(10)
            }
            post {
                always {
                    cleanWs()
                }
            }
        }
        stage('Deploy') {
            agent { label "my127ws-preview" }
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
                sh 'env | sort -n'
            }
            post {
                always {
                    cleanWs()
                }
            }
        }
    }
}
