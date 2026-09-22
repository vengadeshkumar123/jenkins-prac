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
    }

    post {

        success {
            echo "Pipeline completed successfully!"
        }

        failure {
            echo "Pipeline failed!"
        }

        always {
            echo "Execution completed."
        }
    }
}
