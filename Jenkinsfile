pipeline {
    agent any

    environment {
        GIT_REPO   = 'https://github.com/bharathsavadatti447/docker-ecr.git'
        BRANCH     = 'main'
        AWS_REGION = 'us-east-1'
        ECR_REPO   = 'public.ecr.aws/o9v8l7j1/dockerrepo'
        IMAGE_NAME = 'node-app'
        IMAGE_TAG  = 'v1.0.0'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git(
                    branch: "${BRANCH}",
                    url: "${GIT_REPO}",
                    credentialsId: 'a5e5c631-8e24-48ee-9844-ea7c8b7a658d'
                )
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    sh '''
                        echo "Logging into AWS Public ECR..."
                        aws ecr-public get-login-password --region $AWS_REGION | \
                        docker login --username AWS --password-stdin public.ecr.aws/o9v8l7j1
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dir('node-app') {
                        sh '''
                            echo "Building Docker image..."
                            docker build -t $IMAGE_NAME:$IMAGE_TAG .
                            docker tag $IMAGE_NAME:$IMAGE_TAG $ECR_REPO:$IMAGE_TAG
                        '''
                    }
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    sh '''
                        echo "Pushing Docker image to ECR..."
                        docker push $ECR_REPO:$IMAGE_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker image pushed successfully to ECR: $ECR_REPO:$IMAGE_TAG"
        }
        failure {
            echo "❌ Failed to push Docker image to ECR"
        }
    }
}

