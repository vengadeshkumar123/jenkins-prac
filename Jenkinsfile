pipeline {

    agent any

    environment {
        APP_NAME = 'Jenkins Practice App'
        ENVIRONMENT = 'development'
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
                echo "Environment: ${ENVIRONMENT}"
                sh 'chmod +x app.sh'
            }
        }

        stage('Test') {
            steps {
                echo "Testing ${APP_NAME}"
                echo "Environment: ${ENVIRONMENT}"
                sh './app.sh'
            }
        }
    }

    post {

        success {
            echo "Pipeline for ${APP_NAME} completed successfully!"
        }

        failure {
            echo "Pipeline for ${APP_NAME} failed!"
        }

        always {
            echo "Execution completed in ${ENVIRONMENT} environment."
        }
    }
}
