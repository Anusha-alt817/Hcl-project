pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '559401928438.dkr.ecr.us-east-1.amazonaws.com/devops-platform'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building application with Maven...'
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'

                sh """
                    docker build \
                      -t ${ECR_REPO}:${IMAGE_TAG} \
                      -t ${ECR_REPO}:latest .
                """
            }
        }

        stage('ECR Login') {
            steps {
                echo 'Logging in to Amazon ECR...'

                sh """
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS --password-stdin ${ECR_REPO}
                """
            }
        }

        stage('Push to ECR') {
            steps {
                echo 'Pushing Docker image to Amazon ECR...'

                sh """
                    docker push ${ECR_REPO}:${IMAGE_TAG}
                    docker push ${ECR_REPO}:latest
                """
            }
        }

        stage('Success') {
            steps {
                echo "CI/CD pipeline completed successfully. Image: ${ECR_REPO}:${IMAGE_TAG}"
            }
        }
    }
}
```
