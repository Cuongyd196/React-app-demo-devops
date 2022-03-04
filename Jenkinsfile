pipeline {
    agent any

    parameters {
            string(name: 'ImageName', defaultValue: 'cuongyd196/react-app-demo-devops', description: 'This is my image ! ')
            string(name: 'ImageTag', defaultValue: 'latest', description: '')
        }

    environment {
        DOCKERHUB_CREDENTIALS=credentials('DockerHub')
        NAME = 'CUONG NGUYEN'
        GITHUB = 'cuongyd196'
    }
    stages {

        stage('Validate') {
            steps {
                echo "Hello ${NAME}"

                echo "GITHUB: ${GITHUB}"

                echo "ImageName: ${params.ImageName}"

                echo "ImageTag: ${params.ImageTag}"
            }
          }

        stage('Run and Build') {
            agent {
                node {
                    label 'aws_ec2'
                }
            }
            steps {
                echo 'Run and Build aws_ec2'
                echo 'Building  image - ImageName: ${params.ImageName} - ImageTag: ${params.ImageTag}'
                echo "ImageName: ${params.ImageName}"
                echo "${params.ImageName}:${params.ImageTag}"
                sh "sudo docker build -t ${params.ImageName}:${params.ImageTag} ."
            }
        }

        stage('Package and Push') {
            agent {
                node {
                    label 'aws_ec2'
                }
            }
            steps {
                echo 'Start pushing.. with credential'
                sh 'echo $DOCKERHUB_CREDENTIALS'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "sudo docker push ${params.ImageName}:${params.ImageTag}"
            }
        }

        stage('Deploy') {
            agent {
                node {
                    label 'aws_ec2'
                }
            }
            steps {
                echo 'Deploying....'
                sh 'sudo docker container stop react-app-demo-devops || echo "this container does not exist" '
                sh 'sudo docker network create react-app-net || echo "this network exists"'
                sh "sudo docker container run -d --rm --name react-app-demo-devops -p 3333:80 --network react-app-net ${params.ImageName}:${params.ImageTag}"
                telegramSend('Deploying - $PROJECT_NAME – # $BUILD_NUMBER – STATUS: $BUILD_STATUS!')

            }
        }
    }
}