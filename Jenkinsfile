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
            steps {
                echo 'Building  image - ImageName: ${params.ImageName} - ImageTag: ${params.ImageTag}'
                echo "ImageName: ${params.ImageName}"
                sh 'docker build -t ${params.ImageName}:${params.ImageTag} .'
            }
        }

        stage('Package and Push') {
            steps {
                echo 'Start pushing.. with credential'
                sh 'echo $DOCKERHUB_CREDENTIALS'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push ${params.ImageName}:${params.ImageTag}'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh 'docker container stop react-app-demo-devops || echo "this container does not exist" '
                sh 'docker network create react-app-net || echo "this network exists"'
                sh 'docker container run -d --rm --name react-app-demo-devops -p 3333:80 --network react-app-net ${params.ImageName}:${params.ImageTag}'
                telegramSend('Deploying - $PROJECT_NAME – # $BUILD_NUMBER – STATUS: $BUILD_STATUS!')

            }
        }
    }
}