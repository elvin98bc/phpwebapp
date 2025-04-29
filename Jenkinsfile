pipeline {
    agent none  // No global agent, each stage will define its own

    environment {
        DOCKER_CONFIG = '/tmp/.docker'  // Set to a directory with write access
        repoUri = "538774323759.dkr.ecr.ap-southeast-1.amazonaws.com/webform"
        repoRegistryUrl = "https://538774323759.dkr.ecr.ap-southeast-1.amazonaws.com"
        registryCreds = 'ecr:ap-southeast-1:awscreds'
        cluster = "webform"
        service = "webform-svc"
        region = 'ap-southeast-1'
    }

    stages {
        stage('Docker Test') {
            agent {
                docker {
                    image 'docker:latest' // or any Docker CLI image
                    args '-v /certs/client:/certs/client:ro'
                }
            }
            environment {
                DOCKER_HOST = 'tcp://docker:2376'
                DOCKER_CERT_PATH = '/certs/client'
                DOCKER_TLS_VERIFY = '1'
            }
            steps {
                sh 'docker version'
                sh 'docker ps -a'
            }
        }

        // stage('Build Docker Image') {
        //     agent {
        //         docker {
        //             image 'docker:latest'
        //             args '-v /var/run/docker.sock:/var/run/docker.sock'  // Mount Docker socket
        //         }
        //     }
        //     steps {
        //         script {
        //             echo 'Building Docker Image from Dockerfile...'
        //             sh 'mkdir -p /tmp/.docker'  // Ensure the directory exists
        //             dockerImage = docker.build(repoUri + ":$BUILD_NUMBER")
        //         }
        //     }
        // }

        // stage('Push Docker Image to ECR') {
        //     agent {
        //         docker {
        //             image 'docker:latest'
        //             args '-v /var/run/docker.sock:/var/run/docker.sock'  // Mount Docker socket
        //         }
        //     }
        //     steps {
        //         script {
        //             echo "Pushing Docker Image to ECR..."
        //             docker.withRegistry(repoRegistryUrl, registryCreds) {
        //                 dockerImage.push("$BUILD_NUMBER")
        //                 dockerImage.push('latest')
        //             }
        //         }
        //     }
        // }

        // stage('Deploy to ECS') {
        //     agent {
        //         docker {
        //             image 'amazon/aws-cli:latest'  // Use a pre-built AWS CLI Docker image for ECS deployment
        //             args '-v /var/run/docker.sock:/var/run/docker.sock --entrypoint=""'  // Optional if needed by AWS CLI
        //         }
        //     }
        //     steps {
        //         script {
        //             echo "Deploying Image to ECS..."
        //             withAWS(credentials: 'awscreds', region: "${region}") {
        //                 sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
        //             }
        //         }
        //     }
        // }
    }   
}