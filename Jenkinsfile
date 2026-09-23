pipeline {

    agent any

    stages {

        stage('Docker Build & Push') {
            steps {

                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )
                ]) {

                    sh '''
                        echo "$DOCKERHUB_PASSWORD" | docker login \
                            -u "$DOCKERHUB_USERNAME" \
                            --password-stdin

                        docker build \
                            -t "$DOCKERHUB_USERNAME/jenkins-practice:latest" .

                        docker push \
                            "$DOCKERHUB_USERNAME/jenkins-practice:latest"

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo 'EC2 deployment will be added next'
            }
        }
    }

    post {

        success {
            echo 'CI/CD pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD pipeline failed!'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
