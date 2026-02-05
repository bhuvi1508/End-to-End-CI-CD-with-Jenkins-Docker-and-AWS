pipeline {
    agent any

    environment {
        IMAGE_NAME = 'pwdx/nextjs-app'
        SERVER_IP  = '100.24.113.21'   // IP ONLY (no user here)
    }

    stages {

        stage("Checkout code") {
            steps {
                echo "Checking out code from repository..."
                checkout scm
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                script {
                    echo "Building and pushing Docker image..."

                    withCredentials([
                        usernamePassword(
                            credentialsId: 'dockerHub',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )
                    ]) {
                        sh """
                        docker build -t ${IMAGE_NAME}:latest .
                        echo "${DOCKER_PASS}" | docker login -u "${DOCKER_USER}" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                        docker logout
                        """
                    }
                }
            }
        }

        stage("Deploy to AWS") {
            steps {
                script {
                    echo "Deploying Docker image to AWS EC2..."

                    withCredentials([
                        usernamePassword(
                            credentialsId: 'app.server',
                            usernameVariable: 'SERVER_USER',
                            passwordVariable: 'SERVER_PASS'
                        )
                    ]) {
                        sh """
                        mkdir -p ~/.ssh
                        ssh-keyscan -H ${SERVER_IP} >> ~/.ssh/known_hosts

                        sshpass -p "${SERVER_PASS}" scp docker-compose.yaml \
                            ${SERVER_USER}@${SERVER_IP}:/${SERVER_USER}/docker-compose.yaml

                        sshpass -p "${SERVER_PASS}" ssh -o StrictHostKeyChecking=no \
                            ${SERVER_USER}@${SERVER_IP} '
                            docker pull ${IMAGE_NAME}:latest &&
                            docker-compose -f /${SERVER_USER}/docker-compose.yaml up -d
                            '
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline successfully deployed the app to AWS"
        }
        failure {
            echo "❌ Pipeline failed to deploy the app to AWS"
        }
    }
}
