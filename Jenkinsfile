pipeline {
    agent any

    parameters {
            string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

            text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

            booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

            choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

            password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')

            string(name: 'MYNAME', defaultValue: 'BI', description: 'my name is BI')
        }

    environment {
        DOCKERHUB_CREDENTIALS=credentials('DockerHub')
        NAME = 'CUONG'
    }
    stages {

        stage('Initialize...') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
                echo "Myname is : ${params.MYNAME}"
                sh 'whoami'
                sh 'pwd'
                sh 'newvariable=${params.NAME}'
                sh 'echo $newvariable'
            }
          }

        stage('Build') {
            steps {
                echo "NAME: $NAME"
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
                sh 'docker container run -d --rm --name react-app-demo-devops -p 3333:80 --network react-app-net cuongyd196/react-app-demo-devops'
                telegramSend('Deploying - $PROJECT_NAME – # $BUILD_NUMBER – STATUS: $BUILD_STATUS!')

            }
        }
    }
}