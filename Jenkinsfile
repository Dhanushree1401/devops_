pipeline {
    agent any

    environment {
        IMAGE_NAME = "techtalkjervin/my-app"
        REGISTRY = "docker.io"
        DOCKER_CREDENTIALS_ID = "docker-hub-creds"
        GITHUB_CREDENTIALS_ID = "github-creds"
        APP_DIR = "/opt/docker-kec"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: "${GITHUB_CREDENTIALS_ID}", url: 'https://github.com/Dhanushree1401/devops_', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:latest ."
                }
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'Dhanushree1401', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                    }
                }
            }
        }

        stage('Push Image to Docker Registry') {
            steps {
                script {
                    sh "docker push $IMAGE_NAME:latest"
                }
            }
        }

        stage('Deploy using Docker Compose') {
            steps {
                script {
                    sh """
                        if [ -d "$APP_DIR" ]; then
                            cd $APP_DIR
                            docker-compose pull
                            docker-compose up -d --remove-orphans
                        else
                            echo "Error: Directory $APP_DIR does not exist."
                            exit 1
                        fi
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed! Check the logs for errors.'
        }
    }
}
