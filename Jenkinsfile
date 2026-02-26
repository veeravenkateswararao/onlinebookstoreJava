pipeline {
    agent {
        label 'node1'
    }

    stages {
        stage('get code from git') {
            steps {
               checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'venkygit', url: 'https://github.com/veeravenkateswararao/onlinebookstoreJava.git']])
            }
        }
        stage('validate') {
            steps {
               sh 'mvn validate'
            }
        }
       stage('compile') {
            steps {
               sh 'mvn compile'
            }
        }
        stage('Create File') {
            steps {
        script {
            echo "Creating deployment log"
            sh 'echo "Build Successful" > deploy.log'
        }
    }
}
       stage('test') {
            steps {
               sh 'mvn test'
            }
        }
       stage('package') {
            steps {
               sh 'mvn package'
            }
        }
    }
}
