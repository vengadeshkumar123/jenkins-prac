pipeline {

    agent any

    parameters {
        choice(
            name: 'DEPLOY_ENV',
            choices: ['development', 'staging', 'production'],
            description: 'Select the environment'
        )
    }

    environment {
        APP_NAME = 'Jenkins Practice App'
    }

    options {
        timeout(time: 5, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
                echo "Environment: ${params.DEPLOY_ENV}"
                sh 'chmod +x app.sh'
            }
        }

        stage('Test') {
            steps {
                echo "Testing ${APP_NAME}"
                echo "Environment: ${params.DEPLOY_ENV}"
                sh './app.sh'
            }
        }

        stage('Development Check') {
            when {
                expression {
                    params.DEPLOY_ENV == 'development'
                }
            }
            steps {
                echo 'Running development-specific stage'
            }
        }

        stage('Staging Check') {
            when {
                expression {
                    params.DEPLOY_ENV == 'staging'
                }
            }
            steps {
                echo 'Running staging-specific stage'
            }
        }

        stage('Production Check') {
            when {
                expression {
                    params.DEPLOY_ENV == 'production'
                }
            }
            steps {
                echo 'Running production-specific stage'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Do you want to continue?', ok: 'Proceed'
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }

        always {
            echo 'Execution completed.'
        }
    }
}
