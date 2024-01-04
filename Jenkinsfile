pipeline {
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID = "268320248602"
        AWS_DEFAULT_REGION = "eu-west-2"
        IMAGE_REPO_NAME = "docker-class"
        IMAGE_TAG = "${env.BUILD_ID}"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }

    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY', credentialsId: 'aws_access_key']]) {
                        sh """aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"""
                    }
                }
            }
        }

        stage('Building docker image') {
            steps {
                script {
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Pushing to ECR') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY', credentialsId: 'aws_access_key']]) {
                        sh """docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:${IMAGE_TAG}"""
                        sh """docker push ${REPOSITORY_URI}:${IMAGE_TAG}"""
                    }
                }
            }
        }
    }
}

      //   stage('Deploy to Docker-Server Via SSH') {
      //     steps{
      // sshCommand remote: remote, command: "ls -lrt"
      // sshCommand remote: remote, command: """aws ecr --profile docker-user get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 112843911832.dkr.ecr.eu-west-2.amazonaws.com"""
      // sshCommand remote: remote, command: "docker pull 112843911832.dkr.ecr.eu-west-2.amazonaws.com/docker-repo:5"
      // sshCommand remote: remote, command: "docker run -d -p 9090:80 --name webapp 112843911832.dkr.ecr.eu-west-2.amazonaws.com/docker-repo:5"
      // }
      // } 


  