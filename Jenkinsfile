pipeline {
    agent any

    environment{
        IMAGE_NAME = "javabook"
        IMAGE_TAG = "v1"
        DOCKER_USER = "venkyveera"
    }

    stages {

        stage('Get Code from Git') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        credentialsId: 'venkygit',
                        url: 'https://github.com/veeravenkateswararao/onlinebookstoreJava.git'
                    ]]
                )
            }
        }

        stage('Validate') {
            steps {
                sh 'mvn validate'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'Docker_CRED',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                docker service update --image $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG bookstore || docker service create --name bookstore --replicas 3 -p 8022:8080 $DOCKER_USER/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }

    }
}
