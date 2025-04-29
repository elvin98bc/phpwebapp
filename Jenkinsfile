pipeline {
    agent any

    environment {
        DOCKER_CONFIG = '/tmp/.docker'
        repoUri = "538774323759.dkr.ecr.ap-southeast-1.amazonaws.com/webform"
        repoRegistryUrl = "https://538774323759.dkr.ecr.ap-southeast-1.amazonaws.com"
        registryCreds = 'ecr:ap-southeast-1:awscreds'
        cluster = "webform"
        service = "webform-svc"
        region = 'ap-southeast-1'
    }

    stages {
        stage('Docker Test') {
            steps {
                sh 'docker ps -a'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker Image from Dockerfile...'
                    sh 'mkdir -p /tmp/.docker'  // Ensure the directory exists
                    dockerImage = docker.build(repoUri + ":$BUILD_NUMBER")
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    echo "Pushing Docker Image to ECR..."
                    docker.withRegistry(repoRegistryUrl, registryCreds) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to ECS') {
            agent {
                docker {
                    image 'aws-jenkins-agent:latest'
                    args '-u root'
                }
            }
            steps {
                withAWS(credentials: 'awscreds', region: "${region}") {
                    sh """
                        aws ecs update-service \
                        --cluster ${cluster} \
                        --service ${service} \
                        --force-new-deployment
                    """
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    echo 'Cleaning up...'
                    sh 'docker rmi -f $repoUri:$BUILD_NUMBER'
                }
            }
        }
    }
}