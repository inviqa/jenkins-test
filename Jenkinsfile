pipeline {
    agent { label "my127ws-preview" }
    environment {
        COMPOSE_DOCKER_CLI_BUILD = 1
        DOCKER_BUILDKIT = 1
        MY127WS_ENV = "pipeline"
    }
    triggers { cron(env.BRANCH_NAME == 'develop' ? 'H H(0-6) * * *' : '') }
    stages {

        stage('Setup') {
            steps {
                sh 'ws harness download'
                sh 'ws harness prepare'
                sh 'ws enable console'
                milestone(10)
            }
        }
        stage('Test')  {
            parallel {
                stage('helm kubeval app')  { steps { sh 'ws helm kubeval --cleanup app' } }
            }
        }
        stage('Build') {
            steps {
                sh 'ws enable'
                milestone(20)
            }
        }

    }
    post {
        always {
            sh 'ws destroy'
            cleanWs()
        }
    }
}
