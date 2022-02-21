pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS=credentials('DockerHub')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building  image..'
                sh 'docker build -t cuongyd196/react-app-demo-devops .'

            }
        }
        stage('Pushing image') {
            steps {
                echo 'Start pushing.. with credential'
                sh 'echo $DOCKERHUB_CREDENTIALS'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push cuongyd196/react-app-demo-devops'

            }
        }
        stage('Deploying') {
            steps {
                echo 'Deploying....'
                sh 'docker container stop react-app-demo-devops || echo "this container does not exist" '
                sh 'docker network create react-app-net || echo "this network exists"'
                sh 'docker container run -d --rm --name react-app-demo-devops -p 3333:3000 --network react-app-net cuongyd196/react-app-demo-devops'
            }
        }
    }
}