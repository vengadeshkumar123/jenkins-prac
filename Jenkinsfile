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

                sh '''
                    aws ssm send-command \
                        --region ap-south-1 \
                        --document-name "AWS-RunShellScript" \
                        --targets "Key=InstanceIds,Values=i-0ef44a9c329b12324" \
                        --parameters 'commands=[
                            "docker pull vengadeshkumar/jenkins-practice:latest",
                            "docker stop jenkins-practice || true",
                            "docker rm jenkins-practice || true",
                            "docker run -d --name jenkins-practice -p 80:80 vengadeshkumar/jenkins-practice:latest"
                        ]'
                '''
            }
        }
    }

    post {

        success {
            echo 'CI/CD deployment completed successfully!'
        }

        failure {
            echo 'CI/CD deployment failed!'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
