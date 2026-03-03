pipeline {
    agent any
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
