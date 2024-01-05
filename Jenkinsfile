pipeline {
    agent any

    environment {
        VERSION = "${env.BUILD_ID}"
        AWS_DEFAULT_REGION = "eu-west-2"
        IMAGE_REPO_NAME = "docker-class"
        IMAGE_TAG = "${env.BUILD_ID}"
        REPOSITORY_URI = "268320248602.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }

    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'aws_access_key_id', variable: 'AWS_ACCESS_KEY_ID'),
                                    string(credentialsId: 'aws_secret_access_key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${REPOSITORY_URI}"
                    }
                }
            }
        }

        stage('Building docker image') {
            steps {
                script {
                    docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Pushing to ECR') {
            steps {
                script {
                    sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:${IMAGE_TAG}"
                    sh "docker push ${REPOSITORY_URI}:${IMAGE_TAG}"
                }
            }
        }

        // Add other stages as needed
    }
}
