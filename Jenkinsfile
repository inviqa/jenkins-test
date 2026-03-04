pipeline {
    agent { label 'linux-arm64-preview' }
    environment {
        MY127WS_ENV = "pipeline"
    }
    stages {
        stage('OS Check') {
            steps {
                sh 'cat /etc/os-release | grep PRETTY_NAME'
                sh 'java -version'
                sh 'docker --version'
                sh 'php -v'
            }
        }
        stage('Build Test') {
            steps {
                sh 'echo "Testing basic functionality..."'
                sh 'docker run --rm hello-world'
            }
        }
        stage('Build') {
            agent { label "my127ws" }
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
